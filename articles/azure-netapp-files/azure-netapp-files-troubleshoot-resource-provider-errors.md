---
title: Fouten bij het oplossen van problemen met Azure NetApp Files Resource providers | Microsoft Docs
description: Beschrijft oorzaken, oplossingen en tijdelijke oplossingen voor veelvoorkomende fouten in Azure NetApp Files van resource providers.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: b-juche
ms.openlocfilehash: f417d83a67f2f3afa33a83a56a72d0d82c64ab0d
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850011"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Problemen met Azure NetApp Files-resourceproviders oplossen 

In dit artikel worden veelvoorkomende fouten van Azure NetApp Files Resource providers, de oorzaken, oplossingen en tijdelijke oplossingen beschreven (indien beschikbaar).

## <a name="common-azure-netapp-files-resource-provider-errors"></a>Veelvoorkomende fouten in Azure NetApp Files Resource provider

***BareMetalTenantId kan niet worden gewijzigd.***  

Deze fout treedt op wanneer u een volume probeert bij te werken of te `BaremetalTenantId` patchen en de eigenschap een gewijzigde waarde heeft.

* Oorzaak:   
U probeert een volume bij te werken en de `BaremetalTenantId` eigenschap heeft een andere waarde dan de waarde die is opgeslagen in Azure.
* Oplossen   
Neem `BaremetalTenantId` niets op in de patch-en update-aanvraag (put). U kunt er ook voor `BaremetalTenantId` zorgen dat hetzelfde is in de aanvraag.

***ServiceLevel kan niet worden gewijzigd.***  

Deze fout treedt op wanneer u een capaciteits groep probeert bij te werken of te patchen met een ander service niveau wanneer de capaciteits pool al volumes bevat.

* Oorzaak:   
U probeert een service niveau voor de capaciteits groep bij te werken wanneer de groep volumes bevat.
* Oplossen   
Verwijder alle volumes uit de capaciteits groep en wijzig vervolgens het service niveau.
* Enkele   
Maak een andere capaciteits groep en maak de volumes opnieuw in de nieuwe capaciteits groep.

***PoolId kan niet worden gewijzigd***  

Deze fout treedt op wanneer u een capaciteits groep wilt bijwerken of een patch wilt `PoolId` uitvoeren met een gewijzigde eigenschap.

* Oorzaak:   
U probeert een eigenschap van een capaciteits `PoolId` groep bij te werken. De `PoolId` eigenschap is een alleen-lezen eigenschap en kan niet worden gewijzigd.
* Oplossen   
Neem `PoolId` niets op in de patch-en update-aanvraag (put).  U kunt er ook voor `PoolId` zorgen dat hetzelfde is in de aanvraag.

***CreationToken kan niet worden gewijzigd.***

Deze fout treedt op wanneer u probeert het bestandspad (`CreationToken`) te wijzigen nadat het volume is gemaakt. Het bestandspad (`CreationToken`) moet worden ingesteld wanneer het volume wordt gemaakt en kan later niet meer worden gewijzigd.

* Oorzaak:   
U probeert het bestandspad (`CreationToken`) te wijzigen nadat het volume is gemaakt. Dit is een niet-ondersteunde bewerking. 
* Oplossen   
Als het wijzigen van het bestandspad niet nodig is, kunt u de para meter verwijderen uit de aanvraag om het fout bericht te sluiten.
* Enkele   
Als u het bestandspad (`CreationToken`) moet wijzigen, kunt u een nieuw volume maken met een nieuw bestandspad en vervolgens de gegevens migreren naar het nieuwe volume.

***CreationToken moet ten minste 16 tekens lang zijn.***

Deze fout treedt op wanneer het bestandspad (`CreationToken`) niet voldoet aan de lengte vereiste. De lengte van het bestandspad moet ten minste één teken lang zijn.

* Oorzaak:   
Het bestandspad is leeg.  Wanneer u een volume maakt met behulp van de API, is een aanmaak token vereist. Als u de Azure Portal gebruikt, wordt het bestandspad automatisch gegenereerd.
* Oplossen   
Voer ten minste één teken in als het bestandspad (`CreationToken`).

***De domein naam kan niet worden gewijzigd.***

Deze fout treedt op wanneer u de domein naam in Active Directory probeert te wijzigen.

* Oorzaak:   
U probeert de eigenschap domain name bij te werken.
* Oplossen    
Geen. U kunt de domein naam niet wijzigen.
* Enkele   
Verwijder alle volumes met behulp van de configuratie van Active Directory. Verwijder vervolgens de Active Directory configuratie en maak de volumes opnieuw.

***Fout met dubbele waarde voor object ExportPolicy. rules [RuleIndex].***

Deze fout treedt op wanneer het export beleid niet is gedefinieerd met een unieke index. Wanneer u een export beleid definieert, moeten alle export beleids regels een unieke index tussen 1 en 5 hebben.

* Oorzaak:   
Het gedefinieerde export beleid voldoet niet aan de vereisten voor het exporteren van beleids regels. U moet één beleids regel voor exporteren hebben met de minimale en vijf export beleids regels met het maximum.
* Oplossen   
Zorg ervoor dat de index nog niet wordt gebruikt en dat deze zich in het bereik van 1 tot 5 bevindt.
* Enkele   
Gebruik een andere index voor de regel die u wilt instellen.

***Fout {Action} {resourceTypeName}***

Deze fout wordt weer gegeven wanneer een andere fout afhandeling de fout niet heeft verwerkt tijdens het uitvoeren van een actie op een resource.   De tekst ' fout ' bevat. Dit `{action}` kan een van de zijn`getting`( `creating`, `updating`,, `deleting`of).  De `{resourceTypeName}` is de `resourceTypeName` `netAppAccount`( bijvoorbeeld,,,enzovoort).`capacityPool` `volume`

* Oorzaak:   
Deze fout is een onverwerkte uitzonde ring waarbij de oorzaak niet bekend is.
* Oplossen   
Neem contact op met het ondersteunings centrum van Azure om de gedetailleerde reden in Logboeken te melden.
* Enkele   
Geen.

***De naam van het bestandspad mag alleen letters, cijfers en afbreek streepjes (""-"") bevatten.***

Deze fout treedt op wanneer het bestandspad niet-ondersteunde tekens bevat, bijvoorbeeld een punt ("."), een komma (","), een onderstrepings teken ("_") of een dollar teken ("$").

* Oorzaak:   
Het bestandspad bevat niet-ondersteunde tekens, bijvoorbeeld een punt ("."), komma (","), onderstrepings teken ("_") of het dollar teken ("$").
* Oplossen   
Verwijder tekens die geen alfabetische letters, cijfers of afbreek streepjes ('-') bevatten uit het bestandspad dat u hebt ingevoerd.
* Enkele   
U kunt een onderstrepings teken vervangen door een koppel teken of een hoofd letter gebruiken in plaats van spaties om het begin van nieuwe woorden aan te geven.  Gebruik bijvoorbeeld ' NewVolume ' in plaats van ' New volume '.

***FileSystemId kan niet worden gewijzigd.***

Deze fout treedt op wanneer u probeert te `FileSystemId`wijzigen.  Wijzigen `FileSystemdId` is geen ondersteunde bewerking. 

* Oorzaak:   
De ID van het bestands systeem wordt ingesteld wanneer het volume wordt gemaakt. `FileSystemId`kan later niet worden gewijzigd.
* Oplossen   
Neem `FileSystemId` niets op in een patch-en update-aanvraag (put).  U kunt er ook voor `FileSystemId` zorgen dat hetzelfde is in de aanvraag.

***ActiveDirectory met id: {string} bestaat niet.***

Het `{string}` gedeelte is de waarde die u hebt ingevoerd `ActiveDirectoryId` in de eigenschap voor de Active Directory verbinding.

* Oorzaak:   
Wanneer u een account met de configuratie van Active Directory hebt gemaakt, hebt u een waarde `ActiveDirectoryId` ingevoerd die moet leeg zijn.
* Oplossen   
Neem `ActiveDirectoryId` niet op in de aanvraag maken (put).

***Ongeldige API-versie.***

De API-versie is niet verzonden of bevat een ongeldige waarde.

* Oorzaak:   
De waarde in de query parameter `api-version` bevat een ongeldige waarde.
* Oplossen   
Gebruik de juiste API-versie waarde.  De resource provider ondersteunt veel API-versies. De waarde heeft de notatie JJJJ-MM-DD.

***Er is een ongeldige waarde {Value} ontvangen voor {1}.***

Dit bericht geeft aan dat er een fout `RuleIndex` `UnixReadOnly`is opgetreden in de `Nfsv3`velden voor `Nfsv4`, `AllowedClients` `UnixReadWrite`,,, en.

* Oorzaak:   
De invoer validatie aanvraag is mislukt voor ten minste een van de volgende velden: `RuleIndex`, `AllowedClients`, `UnixReadOnly`, `UnixReadWrite`, `Nfsv`3 en `Nfsv4`.
* Oplossen   
Zorg ervoor dat u alle vereiste en niet-conflicterende para meters op de opdracht regel instelt. U kunt bijvoorbeeld niet zowel de `UnixReadOnly` als `UnixReadWrite` -para meters op hetzelfde moment instellen.
* Enkele   
Zie de bovenstaande oplossing.

***IP- {0} bereik {1} voor VLAN {2} wordt al gebruikt***

Deze fout treedt op omdat de interne records van het gebruikte IP-bereik een conflict veroorzaken met het zojuist toegewezen IP-adres.

* Oorzaak:   
Het IP-adres dat is toegewezen voor het maken van het volume is al geregistreerd.
De reden kan een eerder mislukt volume zijn.
* Oplossen   
Neem contact op met Azure-ondersteunings centrum.

***Ontbrekende waarde voor {Property}.***

Deze fout geeft aan dat een vereiste eigenschap ontbreekt in de aanvraag. De teken reeks {Property} bevat de naam van de ontbrekende eigenschap.

* Oorzaak:   
De invoer validatie aanvraag is voor ten minste een van de eigenschappen mislukt.
* Oplossen   
Zorg ervoor dat u alle vereiste en niet-conflicterende eigenschappen instelt in de aanvraag, speciaal, de eigenschap van het fout bericht.

***MountTargets kan niet worden gewijzigd.***

Deze fout treedt op wanneer een gebruiker de eigenschap volume MountTargets probeert bij te werken of te patchen.

* Oorzaak:   
U probeert de volume `MountTargets` -eigenschap bij te werken. Het wijzigen van deze eigenschap wordt niet ondersteund.
* Oplossen   
Neem `MountTargets` niets op in een patch-en update-aanvraag (put).  U kunt er ook voor zorgen `MountTargets` dat hetzelfde is in de aanvraag.

***De naam wordt al gebruikt.***

Deze fout geeft aan dat de naam van de resource al in gebruik is.

* Oorzaak:   
U probeert een resource te maken met een naam die wordt gebruikt voor een bestaande resource.
* Oplossen   
Gebruik een unieke naam bij het maken van de resource.

***Het bestandspad wordt al gebruikt.***

Deze fout geeft aan dat het bestandspad voor het volume al in gebruik is.

* Oorzaak:   
U probeert een volume te maken met een bestandspad dat gelijk is aan het bestaande volume.
* Oplossen   
Gebruik een uniek bestandspad bij het maken van het volume.

***De naam is te lang.***

Deze fout geeft aan dat de resource naam niet voldoet aan de vereiste voor de maximum lengte.

* Oorzaak:   
De resource naam is te lang.
* Oplossen   
Gebruik een kortere naam voor de resource.

***Het bestandspad is te lang.***

Deze fout geeft aan dat het bestandspad voor het volume niet voldoet aan de vereiste voor de maximum lengte.

* Oorzaak:   
Het pad naar het volume bestand is te lang.
* Oplossen   
Gebruik een kortere bestands locatie.

***De naam is te kort.***

Deze fout geeft aan dat de resource naam niet voldoet aan de vereiste voor de minimum lengte.

* Oorzaak:   
De resource naam is te kort.
* Oplossen   
Gebruik een langere naam voor de resource.

***Het bestandspad is te kort.***

Deze fout geeft aan dat het pad naar het volume bestand niet voldoet aan de vereiste voor de minimum lengte.

* Oorzaak:   
Het pad naar het volume bestand is te kort.
* Oplossen   
Verg root de lengte van het pad naar het volume bestand.

***De Azure NetApp Files-API is onbereikbaar.***

De Azure-API is afhankelijk van de Azure NetApp Files-API voor het beheren van volumes. Deze fout wijst op een probleem met de API-verbinding.

* Oorzaak:   
De onderliggende API reageert niet, wat resulteert in een interne fout. Deze fout is waarschijnlijk tijdelijk.
* Oplossen   
Het probleem is waarschijnlijk tijdelijk. De aanvraag moet na enige tijd slagen.
* Enkele   
Geen. De onderliggende API is essentieel voor het beheren van volumes.

***Er is{0}geen resultaat-id van de bewerking gevonden voor.***

Deze fout geeft aan dat de bewerking niet kan worden voltooid vanwege een interne fout.

* Oorzaak:   
Er is een interne fout opgetreden waardoor de bewerking niet kan worden voltooid.
* Oplossen   
Deze fout is waarschijnlijk tijdelijk. Wacht een paar minuten en probeer het opnieuw. Als het probleem zich blijft voordoen, maakt u een ticket voor technische ondersteuning om het probleem te onderzoeken.
* Enkele   
Wacht een paar minuten en controleer of het probleem zich blijft voordoen.

***Het is niet toegestaan om protocol typen CIFS en NFS te combi neren***

Deze fout treedt op wanneer u een volume probeert te maken en er zijn beide typen CIFS (SMB) en NFS-protocol in de volume-eigenschappen.

* Oorzaak:   
De protocol typen CIFS (SMB) en NFS worden gebruikt in de volume-eigenschappen.
* Oplossen   
Verwijder een van de protocol typen.
* Enkele   
Laat de eigenschap protocol type leeg of null.

***Aantal items: {value} voor object: ExportPolicy. rules [RuleIndex] bevindt zich buiten het minimum bereik.***

Deze fout treedt op wanneer de export beleids regels niet voldoen aan het minimum-of maximum bereik vereiste. Als u het export beleid definieert, moet er één beleids regel voor het exporteren van de minimum-en vijf export beleids regels zijn ingesteld op het maximum.

* Oorzaak:   
Het export beleid dat u hebt gedefinieerd, voldoet niet aan het vereiste bereik.
* Oplossen   
Zorg ervoor dat de index niet al wordt gebruikt en dat deze zich in het bereik van 1 tot 5 bevindt.
* Enkele   
Het is niet verplicht om export beleid te gebruiken op de volumes. U kunt het export beleid volledig weglaten als u geen export beleids regels hoeft te gebruiken.

***Er is slechts één Active Directory toegestaan***

Deze fout treedt op wanneer u een Active Directory configuratie probeert te maken en er al een bestaat voor het abonnement in de regio. De fout kan ook optreden wanneer u probeert meer dan een Active Directory configuratie te maken.

* Oorzaak:   
U probeert een Active Directory te maken (niet bij te werken), maar er bestaat al een.
* Oplossen   
Als de Active Directory configuratie niet wordt gebruikt, kunt u eerst de bestaande configuratie verwijderen en de bewerking opnieuw uitvoeren.
* Enkele   
Geen. Er is slechts één Active Directory toegestaan.

***De bewerking {Operation} wordt niet ondersteund.***

Deze fout geeft aan dat de bewerking niet beschikbaar is voor het actieve abonnement of de resource.

* Oorzaak:   
De bewerking is niet beschikbaar voor het abonnement of de resource.
* Oplossen   
Zorg ervoor dat de bewerking correct is ingevoerd en dat deze beschikbaar is voor de resource en het abonnement die u gebruikt.

***OwnerId kan niet worden gewijzigd***

Deze fout treedt op wanneer u de eigenschap OwnerId van het volume probeert te wijzigen. Het wijzigen van de OwnerId is geen ondersteunde bewerking. 

* Oorzaak:   
De `OwnerId` eigenschap wordt ingesteld wanneer het volume wordt gemaakt. De eigenschap kan later niet worden gewijzigd.
* Oplossen   
Neem `OwnerId` niets op in een patch-en update-aanvraag (put). U kunt er ook voor zorgen `OwnerId` dat hetzelfde is in de aanvraag.

***Bovenliggende groep niet gevonden***

Deze fout treedt op wanneer u probeert een volume te maken en de capaciteits pool waarin u het volume maakt, niet is gevonden.

* Oorzaak:   
De capaciteits groep waar het volume wordt gemaakt, is niet gevonden.
* Oplossen   
Waarschijnlijk is de groep niet volledig gemaakt of is deze al verwijderd op het moment dat het volume werd gemaakt.

***De patch bewerking wordt niet ondersteund voor dit bron type.***

Deze fout treedt op wanneer u probeert het koppel doel of de moment opname te wijzigen.

* Oorzaak:   
Het koppel doel wordt gedefinieerd wanneer het wordt gemaakt en kan daarna niet meer worden gewijzigd.
De moment opnamen bevatten geen eigenschappen die kunnen worden gewijzigd.
* Oplossen   
Geen. Deze resources hebben geen eigenschappen die kunnen worden gewijzigd.

***De groeps grootte is te klein voor de totale volume grootte.***

Deze fout treedt op wanneer u de grootte van de capaciteits pool bijwerkt en de grootte kleiner `usedBytes` is dan de totale waarde van alle volumes in die capaciteits groep.  Deze fout kan ook optreden wanneer u een nieuw volume maakt of het formaat van een bestaand volume wijzigt en de nieuwe grootte van het volume groter is dan de beschik bare ruimte in de capaciteits groep.

* Oorzaak:   
U probeert de capaciteits groep bij te werken naar een kleinere grootte dan usedBytes in alle volumes van de capaciteits groep.  Of u probeert een volume te maken dat groter is dan de beschik bare ruimte in de capaciteits groep.  U kunt ook de grootte van een volume wijzigen en de nieuwe grootte overschrijdt de beschik bare ruimte in de capaciteits groep.
* Oplossen   
Stel de grootte van de capaciteits pool in op een grotere waarde of maak een kleiner volume voor een volume.
* Enkele   
Verwijder voldoende volumes zodat de grootte van de capaciteits groep kan worden bijgewerkt naar deze grootte.

***De eigenschap: De locatie voor de moment opname moet hetzelfde zijn als het volume***

Deze fout treedt op wanneer u een moment opname maakt met een andere locatie dan het volume dat eigenaar is van de moment opname.

* Oorzaak:   
Ongeldige waarde in de locatie-eigenschap voor de moment opname.
* Oplossen   
Stel een geldige teken reeks in voor de eigenschap location.

***De naam van {resource type} moet gelijk zijn aan de naam van de resource-id.***

Deze fout treedt op wanneer u een resource maakt en u de eigenschap name invult met een andere waarde dan de eigenschap name van `resourceId`.

* Oorzaak:   
Ongeldige waarde in de eigenschap name bij het maken van een resource.
* Oplossen   
Laat de eigenschap name leeg of laat het toe om dezelfde waarde te gebruiken als de eigenschap naam (tussen de laatste back slash "/" en het vraag teken "?" `resourceId`) in.

***Het protocol type {Value} is niet bekend***

Deze fout treedt op wanneer u een volume met een onbekend protocol type maakt.  Geldige waarden zijn "NFSv3" en "CIFS".

* Oorzaak:   
U probeert een ongeldige waarde in te stellen in de eigenschap `protocolType` volume.
* Oplossen   
Stel een geldige teken reeks `protocolType`in.
* Enkele   
Stel `protocolType` in op null.

***Protocol typen kunnen niet worden gewijzigd***

Deze fout treedt op wanneer u een volume probeert bij `ProtocolType` te werken of te patchen.  Het wijzigen van protocol type is geen ondersteunde bewerking.

* Oorzaak:   
De `ProtocolType` eigenschap wordt ingesteld wanneer het volume wordt gemaakt.  Het kan niet worden bijgewerkt.
* Oplossen   
Geen.
* Enkele   
Maak een ander volume met nieuwe protocol typen.

***Het maken van de resource van het type {resource soort} overschrijdt het quotum van {quota} resources van het type {resource soort} per {parentResourceType}. Het huidige aantal resources is {currentCount}. Verwijder een aantal bronnen van dit type voordat u een nieuw item maakt.***

Deze fout treedt op wanneer u een`NetAppAccount`bron ( `Volume`, `CapacityPool`, of `Snapshot`) probeert te maken, maar het quotum de limiet heeft bereikt.

* Oorzaak:   
U probeert een resource te maken, maar de quotum limiet is bereikt (voor beeld: `NetAppAccounts` per abonnement of `CapacityPools` per `NetAppAccount`).
* Oplossen   
Verhoog de quotum limiet.
* Enkele   
Verwijder ongebruikte resources van hetzelfde type en maak deze opnieuw.

***Er is een waarde ontvangen voor de eigenschap {PropertyName} met het kenmerk alleen-lezen.***

Deze fout treedt op wanneer u een waarde definieert voor een eigenschap die niet kan worden gewijzigd. U kunt de volume-ID bijvoorbeeld niet wijzigen.

* Oorzaak:   
U probeert een para meter (bijvoorbeeld de volume-ID) te wijzigen die niet kan worden gewijzigd.
* Oplossen   
Wijzig geen waarde voor de eigenschap.

***De aangevraagde {resource} is niet gevonden.***

Deze fout treedt op wanneer u probeert te verwijzen naar een niet-bestaande resource, bijvoorbeeld een volume of een moment opname. De resource is mogelijk verwijderd of heeft een speld resource naam.

* Oorzaak:   
U probeert te verwijzen naar een niet-bestaande resource (bijvoorbeeld een volume of moment opname) die al is verwijderd of een verkeerd gespelde resource naam heeft.
* Oplossen   
Controleer de vraag op spel fouten om er zeker van te zijn dat deze correct wordt verwezen.
* Enkele   
Zie de sectie oplossing hierboven.

***Het service niveau {volumeServiceLevel} is hoger dan het bovenliggende {poolServiceLevel}***

Deze fout treedt op wanneer u een volume maakt of bijwerkt en u het service niveau hebt ingesteld op een hoger niveau dan de capaciteits groep die deze bevat.

* Oorzaak:   
U probeert een volume te maken of bij te werken met een hoger geclassificeerd service niveau dan de bovenliggende capaciteits groep.
* Oplossen   
Stel het service niveau in op dezelfde of een lagere positie dan de bovenliggende capaciteits groep.
* Enkele   
Maak het volume in een andere capaciteits pool met een correct service niveau. U kunt ook alle volumes uit de capaciteits groep verwijderen en service niveau voor de capaciteits groep instellen op een hogere positie.

***De naam van de SMB-server mag niet langer zijn dan 10 tekens.***

Deze fout treedt op wanneer u een Active Directory configuratie voor een account maakt of bijwerkt.

* Oorzaak:   
De lengte van de SMB-server naam is langer dan 10 tekens.
* Oplossen   
Gebruik een kortere server naam. De maximale lengte is 10 tekens.
* Enkele   
Geen.  Zie de bovenstaande oplossing. 

***SubnetId kan niet worden gewijzigd.***

Deze fout treedt op wanneer u probeert het `subnetId` volume te wijzigen nadat het is gemaakt.  `SubnetId`moet worden ingesteld wanneer het volume wordt gemaakt en kan later niet meer worden gewijzigd.

* Oorzaak:   
U probeert het `subnetId` te wijzigen nadat het volume is gemaakt. Dit is een niet-ondersteunde bewerking. 
* Oplossen   
Als het `subnetId` niet nodig is om het te wijzigen, kunt u de para meter verwijderen uit de aanvraag om het fout bericht te sluiten.
* Enkele   
Als u het `subnetId`moet wijzigen, kunt u een nieuw volume maken met een nieuwe `subnetId`en vervolgens de gegevens naar het nieuwe volume migreren.

***SubnetId heeft een ongeldige indeling.***

Deze fout treedt op wanneer u een nieuw volume probeert te maken, `subnetId` maar het is `resourceId` geen voor een subnet.

* Oorzaak:   
Deze fout treedt op wanneer u een nieuw volume probeert te maken, maar `subnetId` het is `resourceId` geen voor een subnet. 
* Oplossen   
Controleer de waarde voor de `subnetId` om te controleren of deze een `resourceId` bevat voor het gebruikte subnet.
* Enkele   
Geen. Zie de bovenstaande oplossing. 

***Het subnet moet de overdracht ' micro soft. NetApp/volumes ' hebben.***

Deze fout treedt op wanneer u een volume maakt en het geselecteerde subnet niet wordt overgedragen `Microsoft.NetApp/volumes`.

* Oorzaak:   
U hebt geprobeerd het volume te maken en u hebt een subnet geselecteerd dat niet is `Microsoft.NetApp/volumes`overgedragen.
* Oplossen   
Selecteer een ander subnet dat wordt overgedragen `Microsoft.NetApp/volumes`.
* Enkele   
Voeg een juiste overdracht toe aan het subnet.

***Het opgegeven resource type is onbekend/niet van toepassing.***

Deze fout treedt op wanneer een naam controle is aangevraagd voor een niet-toepasselijk resource type of voor een onbekend resource type.

* Oorzaak:   
Er is een naam controle aangevraagd voor een onbekend of niet-ondersteund resource type.
* Oplossen   
Controleer of de resource waarvoor u de aanvraag uitvoert, wordt ondersteund of geen spel fouten bevat.
* Enkele   
Zie de bovenstaande oplossing.

***Onbekende Azure NetApp Files fout.***

De Azure-API is afhankelijk van de Azure NetApp Files-API voor het beheren van volumes. De fout geeft aan dat er een probleem is in de communicatie met de API.

* Oorzaak:   
De onderliggende API verzendt een onbekende fout. Deze fout is waarschijnlijk tijdelijk.
* Oplossen   
Het probleem is waarschijnlijk tijdelijk en de aanvraag moet na enige tijd slagen. Als het probleem zich blijft voordoen, maakt u een ondersteunings ticket om het probleem te onderzoeken.
* Enkele   
Geen. De onderliggende API is essentieel voor het beheren van volumes.

***Ontvangen waarde voor een onbekende eigenschap {PropertyName}.***

Deze fout treedt op wanneer er niet-bestaande eigenschappen worden gegeven voor een resource zoals het volume, de moment opname of het doel van de koppeling.

* Oorzaak:   
De aanvraag heeft een reeks eigenschappen die kunnen worden gebruikt voor elke resource. U kunt geen niet-bestaande eigenschappen in de aanvraag toevoegen.
* Oplossen   
Zorg ervoor dat alle eigenschapnamen juist zijn gespeld en dat de eigenschappen beschikbaar zijn voor het abonnement en de resource.
* Enkele   
Verminder het aantal eigenschappen dat is gedefinieerd in de aanvraag om de eigenschap te elimineren die de fout veroorzaakt.

***Update-bewerking wordt niet ondersteund voor dit bron type.***

Alleen volumes kunnen worden bijgewerkt. Deze fout treedt op wanneer u een niet-ondersteunde update bewerking probeert uit te voeren, bijvoorbeeld het bijwerken van een moment opname.

* Oorzaak:   
De resource die u wilt bijwerken, biedt geen ondersteuning voor de update bewerking. De eigenschappen van alleen volumes kunnen worden gewijzigd.
* Oplossen   
Geen. De resource die u wilt bijwerken, biedt geen ondersteuning voor de update bewerking. Daarom kan deze niet worden gewijzigd.
* Enkele   
Voor een volume maakt u een nieuwe resource met de update op de locatie en migreert u de gegevens.

***Volume kan niet worden gemaakt in een groep die niet de status geslaagd heeft.***

Deze fout treedt op wanneer u een volume in een groep probeert te maken dat niet de status voltooid heeft. De bewerking voor het maken van de capaciteits groep is waarschijnlijk om een of andere reden mislukt.

* Oorzaak:   
De capaciteits groep met het nieuwe volume heeft de status mislukt.
* Oplossen   
Controleer of de capaciteits groep is gemaakt en niet de status Mislukt heeft.
* Enkele   
Maak een nieuwe capaciteits groep en maak het volume in de nieuwe groep.

***Het volume wordt gemaakt en kan op dit moment niet worden verwijderd.***

Deze fout treedt op wanneer u een volume probeert te verwijderen dat nog wordt gemaakt.

* Oorzaak:   
Er wordt nog een volume gemaakt wanneer u het volume probeert te verwijderen.
* Oplossen   
Wacht tot het maken van het volume is voltooid en probeer het opnieuw.
* Enkele   
Zie de bovenstaande oplossing.

***Het volume wordt verwijderd en kan op dit moment niet worden verwijderd.***

Deze fout treedt op wanneer u een volume probeert te verwijderen wanneer dit al wordt verwijderd.

* Oorzaak:   
Er wordt al een volume verwijderd wanneer u het volume probeert te verwijderen.
* Oplossen   
Wacht tot de huidige verwijderings bewerking is voltooid.
* Enkele   
Zie de bovenstaande oplossing.

***Het volume wordt bijgewerkt en kan op dit moment niet worden verwijderd.***

Deze fout treedt op wanneer u een volume probeert te verwijderen dat wordt bijgewerkt.

* Oorzaak:   
Er wordt een volume bijgewerkt wanneer u het volume probeert te verwijderen.
* Oplossen   
Wacht tot de update bewerking is voltooid en probeer het opnieuw.
* Enkele   
Zie de bovenstaande oplossing.

***Het volume is niet gevonden of is niet gemaakt.***

Deze fout treedt op wanneer het maken van het volume is mislukt en u probeert het volume te wijzigen of een moment opname te maken voor het volume.

* Oorzaak:   
Het volume bestaat niet of het maken is mislukt.
* Oplossen   
Controleer of u het juiste volume wijzigt en het maken van het volume is geslaagd. U kunt ook controleren of het volume waarvoor u een moment opname maakt bestaat.
* Enkele   
Geen.  Zie de bovenstaande oplossing. 

***Het opgegeven token voor maken bestaat al***

Deze fout treedt op wanneer u een volume probeert te maken en u een aanmaak token (exportpad) opgeeft waarvoor al een volume bestaat.

* Oorzaak:   
Het maken van het token (exportpad) dat u hebt opgegeven tijdens het maken van het volume is al gekoppeld aan een ander volume. 
* Oplossen   
Kies een ander aanmaak token.  U kunt ook het andere volume verwijderen.

***Het opgegeven token voor maken is gereserveerd***

Deze fout treedt op wanneer u een volume probeert te maken en u ' default ' of ' none ' opgeeft als het bestandspad (aanmaak token).

* Oorzaak:    
U probeert een volume te maken en u geeft "default" of "none" op als het bestandspad (aanmaak token).
* Oplossen   
Kies een ander bestandspad (token voor maken).
 
***Active Directory referenties worden gebruikt***

Deze fout treedt op wanneer u probeert de Active Directory configuratie te verwijderen van een account waarbij ten minste één SMB-volume nog bestaat.  Het SMB-volume is gemaakt met behulp van de Active Directory configuratie die u probeert te verwijderen.

* Oorzaak:   
U probeert de Active Directory configuratie van een account te verwijderen, maar er is ten minste één SMB-volume nog steeds aanwezig dat oorspronkelijk is gemaakt met behulp van de Active Directory-configuratie. 
* Oplossen   
Verwijder eerst alle SMB-volumes die zijn gemaakt met behulp van de Active Directory configuratie.  Probeer de configuratie vervolgens opnieuw te verwijderen.

***Kan de toewijzing van de organisatie-eenheid niet wijzigen als de referenties in gebruik zijn***

Deze fout treedt op wanneer u de organisatie-eenheid van een Active Directory configuratie probeert te wijzigen, maar er nog ten minste één SMB-volume bestaat.  Het SMB-volume is gemaakt met behulp van die Active Directory configuratie die u probeert te verwijderen.

* Oorzaak:   
U probeert de organisatie-eenheid van een Active Directory configuratie te wijzigen.  Maar er bestaat nog minstens één SMB-volume dat oorspronkelijk is gemaakt met behulp van de Active Directory configuratie.
* Oplossen   
 Verwijder eerst alle SMB-volumes die zijn gemaakt met behulp van de Active Directory configuratie.  Probeer de configuratie vervolgens opnieuw te verwijderen. 

***Active Directory update wordt al uitgevoerd***

Deze fout treedt op wanneer u een Active Directory configuratie probeert te bewerken waarvoor al een bewerkings bewerking wordt uitgevoerd.

* Oorzaak:   
U probeert een Active Directory configuratie te bewerken, maar er wordt al een andere bewerking uitgevoerd.
* Oplossen   
Wacht tot de actieve bewerkings bewerking is voltooid.

***Alle volumes met de geselecteerde referenties eerst verwijderen***

Deze fout treedt op wanneer u een Active Directory configuratie probeert te verwijderen, maar er nog ten minste één SMB-volume bestaat.  Het SMB-volume is gemaakt met behulp van de Active Directory configuratie die u probeert te verwijderen.

* Oorzaak:   
U probeert een Active Directory configuratie te verwijderen, maar er is ten minste één SMB-volume nog aanwezig dat oorspronkelijk is gemaakt met behulp van de Active Directory-configuratie.
* Oplossen   
Verwijder eerst alle SMB-volumes die zijn gemaakt met behulp van de Active Directory configuratie.  Probeer de configuratie vervolgens opnieuw te verwijderen. 

***Er zijn geen Active Directory referenties gevonden in de regio***

Deze fout treedt op wanneer u een SMB-volume probeert te maken, maar er is geen Active Directory configuratie toegevoegd aan het account voor de regio.

* Oorzaak:   
U probeert een SMB-volume te maken, maar er is geen Active Directory configuratie aan het account toegevoegd. 
* Oplossen   
Voeg een Active Directory configuratie toe aan het account voordat u een SMB-volume maakt.

***Kan geen query uitvoeren op DNS-server. Controleer of de netwerk configuratie juist is en of de DNS-servers beschikbaar zijn.***

Deze fout treedt op wanneer u een SMB-volume probeert te maken, maar er is geen DNS-server (opgegeven in uw Active Directory configuratie) onbereikbaar. 

* Oorzaak:   
U probeert een SMB-volume te maken, maar er is geen DNS-server (opgegeven in de Active Directory configuratie) onbereikbaar.
* Oplossen   
Controleer uw Active Directory configuratie en zorg ervoor dat de IP-adressen van de DNS-server juist en bereikbaar zijn.
Als er geen problemen zijn met de IP-adressen van de DNS-server, controleert u of de toegang wordt geblokkeerd door firewalls.

***Te veel gelijktijdige taken***

Deze fout treedt op wanneer u een moment opname probeert te maken wanneer er al drie andere bewerkingen voor het maken van moment opnamen worden uitgevoerd voor het abonnement.

* Oorzaak:   
U probeert een moment opname te maken wanneer er al drie andere bewerkingen voor het maken van moment opnamen worden uitgevoerd voor het abonnement. 
* Oplossen   
Het maken van een moment opname van taken kan een paar seconden duren.  Wacht een paar seconden en voer de bewerking voor het maken van de moment opname opnieuw uit.

***Er kunnen geen extra taken worden geproduceerd. Wacht tot de lopende taken zijn voltooid en probeer het opnieuw***

Deze fout kan optreden wanneer u een volume wilt maken of verwijderen onder specifieke omstandigheden.

* Oorzaak:   
U probeert een volume te maken of verwijderen onder specifieke omstandigheden.
* Oplossen   
Wacht een paar minuten en voer de bewerking opnieuw uit.

***Er wordt al een volume overgang tussen de statussen***

Deze fout kan optreden wanneer u probeert een volume te verwijderen dat zich momenteel in een overgangs status bevindt (dat wil zeggen, momenteel in de status maken, bijwerken of verwijderen).

* Oorzaak:   
U probeert een volume te verwijderen dat zich momenteel in een overgangs status bevindt.
* Oplossen   
Wacht tot de bewerking die actief is (status overgang) is voltooid en voer de bewerking opnieuw uit.

***Het splitsen van het nieuwe volume van de moment opname van het bron volume is mislukt***

 Deze fout kan optreden wanneer u probeert een volume te maken op basis van een moment opname.  

* Oorzaak:   
U probeert een volume te maken op basis van een moment opname en het volume eindigt op een fout status.
* Oplossen   
Verwijder het volume en voer de bewerking voor het maken van het volume uit van de moment opname opnieuw uit.

 
## <a name="next-steps"></a>Volgende stappen

* [Ontwikkel voor Azure NetApp Files met REST API](azure-netapp-files-develop-with-rest-api.md)
