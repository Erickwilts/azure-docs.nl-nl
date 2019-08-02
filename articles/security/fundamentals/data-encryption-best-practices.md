---
title: Best practices voor gegevens beveiliging en-versleuteling-Microsoft Azure
description: Dit artikel bevat een aantal aanbevolen procedures voor gegevens beveiliging en-versleuteling met ingebouwde Azure-functies.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/06/2019
ms.author: terrylan
ms.openlocfilehash: 4df024f9ff37121df4d9be51ae1fa540028ddd3d
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727420"
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Best practices voor gegevensbeveiliging en -versleuteling in Azure
In dit artikel worden aanbevolen procedures beschreven voor gegevens beveiliging en-versleuteling.

De aanbevolen procedures zijn gebaseerd op een advies van de mening en ze werken met de huidige mogelijkheden en functie sets van het Azure-platform. Meningen en technologieën veranderen in de loop van de tijd en dit artikel wordt regel matig bijgewerkt om deze wijzigingen weer te geven.

## <a name="protect-data"></a>Gegevens beveiligen
Als u gegevens in de Cloud wilt beveiligen, moet u rekening houden met de mogelijke statussen waarin uw gegevens zich kunnen voordoen en welke besturings elementen beschikbaar zijn voor die status. Aanbevolen procedures voor Azure-gegevens beveiliging en-versleuteling hebben betrekking op de volgende gegevens statussen:

- Op rest: Dit omvat alle opslag objecten, containers en typen van informatie die statisch bestaan op fysieke media, hetzij magnetische als optische schijf.
- In transit: Wanneer gegevens worden overgedragen tussen onderdelen, locaties of Program ma's, is het onderweg. Voor beelden zijn overdracht via het netwerk, over een service bus (van on-premises naar de Cloud en vice versa, waaronder hybride verbindingen zoals ExpressRoute) of tijdens een invoer-en uitvoer proces.

## <a name="choose-a-key-management-solution"></a>Een oplossing voor sleutel beheer kiezen

Het beveiligen van uw sleutels is essentieel voor het beveiligen van uw gegevens in de Cloud.

[Azure Key Vault](/azure/key-vault/key-vault-overview) helpt bij het beveiligen van cryptografische sleutels en geheimen die worden gebruikt door Cloud toepassingen en-services. Key Vault stroomlijnt het beheerproces voor sleutels en zorgt dat u de controle houdt over de sleutels waarmee uw gegevens toegankelijk zijn en worden versleuteld. Ontwikkel aars kunnen binnen enkele minuten sleutels maken voor ontwikkeling en testen en ze vervolgens migreren naar productie sleutels. Beveiligingsadministrator kunnen wanneer dit nodig is machtigingen aan sleutels verlenen (en intrekken).

U kunt Key Vault gebruiken om meerdere beveiligde containers te maken, ook wel kluizen genoemd. Deze kluizen worden ondersteund door Hsm's. Kluizen verminderen de kans op onbedoeld verlies van beveiligingsinformatie door de opslag van toepassingsgeheimen te centraliseren. Sleutel kluizen regelen en registreren de toegang tot alles wat erin is opgeslagen. Azure Key Vault kunt aanvragen en het vernieuwen van Transport Layer Security (TLS)-certificaten verwerken. Het bevat functies voor een robuuste oplossing voor het beheer van de levens cyclus van certificaten.

Azure Key Vault is ontworpen ter ondersteuning van toepassings sleutels en geheimen. Key Vault is niet bedoeld als een archief voor gebruikers wachtwoorden.

Hieronder vindt u aanbevolen beveiligings procedures voor het gebruik van Key Vault.

**Aanbevolen procedure**: Gebruikers, groepen en toepassingen toegang verlenen tot een specifiek bereik.   
**Details**: Gebruik de vooraf gedefinieerde rollen van RBAC. Als u bijvoorbeeld toegang wilt verlenen aan een gebruiker voor het beheren van sleutel kluizen, wijst u de vooraf gedefinieerde rol [Key Vault bijdrager](/azure/role-based-access-control/built-in-roles) aan deze gebruiker toe aan een bepaald bereik. Het bereik in dit geval is een abonnement, een resource groep of alleen een specifieke sleutel kluis. Als de vooraf gedefinieerde rollen niet aan uw behoeften voldoen, kunt u [uw eigen rollen definiëren](/azure/role-based-access-control/custom-roles).

**Aanbevolen procedure**: Bepalen wat gebruikers toegang hebben tot.   
**Details**: Toegang tot een Key Vault wordt geregeld via twee afzonderlijke interfaces: de beheerlaag en de gegevenslaag. Het toegangsbeheer van de beheerlaag en het toegangsbeheer van de gegevenslaag werken onafhankelijk van elkaar.

Gebruik RBAC om te bepalen welke gebruikers toegang hebben tot. Als u bijvoorbeeld een toepassing toegang wilt verlenen voor het gebruik van sleutels in een sleutel kluis, hoeft u alleen toegangs machtigingen voor het data-vlak te verlenen met behulp van het sleutel kluis toegangs beleid en is er geen beheer vlak toegang nodig voor deze toepassing. Als u daarentegen wilt dat een gebruiker de eigenschappen van de kluis en de tags kan lezen, maar geen toegang heeft tot sleutels, geheimen of certificaten, kunt u deze gebruiker lees toegang verlenen met behulp van RBAC en is er geen toegang nodig tot het gegevens vlak.

**Aanbevolen procedure**: Sla certificaten op in uw sleutel kluis. Uw certificaten zijn van hoge waarde. In de verkeerde handen kan de beveiliging van uw toepassing of de beveiliging van uw gegevens worden aangetast.   
**Details**: Azure Resource Manager kunt certificaten die zijn opgeslagen in Azure Key Vault, veilig implementeren op virtuele Azure-machines wanneer de Vm's worden geïmplementeerd. Door het juiste toegangs beleid voor de sleutel kluis in te stellen, bepaalt u ook wie toegang tot uw certificaat krijgt. Een ander voor deel is dat u al uw certificaten op één plek beheert in Azure Key Vault. Zie [certificaten implementeren op vm's van door de klant beheerde Key Vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) voor meer informatie.

**Aanbevolen procedure**: Zorg ervoor dat u een verwijdering van sleutel kluizen of sleutel kluis objecten kunt herstellen.   
**Details**: Het verwijderen van sleutel kluizen of sleutel kluis objecten kan onbedoeld of schadelijk zijn. Schakel de beveiliging voor het voorlopig verwijderen en opschonen van Key Vault in, met name voor sleutels die worden gebruikt voor het versleutelen van gegevens in rust. Het verwijderen van deze sleutels is gelijk aan gegevens verlies, zodat u zo nodig verwijderde kluizen en kluis objecten kunt herstellen. Oefen Key Vault herstel bewerkingen regel matig uit.

> [!NOTE]
> Als een gebruiker Inzender machtigingen (RBAC) heeft voor een sleutel kluis beheersysteem, kunnen ze zichzelf toegang verlenen tot het gegevens vlak door een sleutel kluis toegangs beleid in te stellen. We raden u aan om nauw keurig te bepalen wie inzenders toegang heeft tot uw sleutel kluizen, om ervoor te zorgen dat alleen geautoriseerde personen uw sleutel kluizen, sleutels, geheimen en certificaten kunnen openen en beheren.
>
>

## <a name="manage-with-secure-workstations"></a>Beheren met beveiligde werk stations

> [!NOTE]
> De abonnements beheerder of de eigenaar moet een beveiligd werk station of een privileged Access-werk station gebruiken.
>
>

Omdat de meeste aanvallen de eind gebruiker bedoelt, wordt het eind punt een van de primaire aanvallen. Een aanvaller die het eind punt verkrijgt, kan de referenties van de gebruiker gebruiken om toegang te krijgen tot de gegevens van de organisatie. De meeste endpoint-aanvallen profiteren van het feit dat gebruikers beheerders zijn op hun lokale werk station.

**Aanbevolen procedure**: Gebruik een veilig beheer werkstation om gevoelige accounts, taken en gegevens te beveiligen.   
**Details**: Gebruik een [privileged Access-werk station](https://technet.microsoft.com/library/mt634654.aspx) om de kwets baarheid op werk stations te verminderen. Deze beveiligde beheer werkstations kunnen u helpen een aantal van deze aanvallen te beperken en ervoor te zorgen dat uw gegevens veiliger zijn.

**Aanbevolen procedure**: Zorg voor Endpoint Protection.   
**Details**: Beveiligings beleid afdwingen voor alle apparaten die worden gebruikt voor het verbruik van gegevens, ongeacht de gegevens locatie (Cloud of on-premises).

## <a name="protect-data-at-rest"></a>Data-at-rest beveiligen

[Gegevens versleuteling in rust](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap voor gegevens privacy, naleving en data soevereiniteit.

**Aanbevolen procedure**: Schijf versleuteling Toep assen om uw gegevens te beveiligen.   
**Details**: Gebruik [Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview). IT-beheerders kunnen de VM-schijven van Windows en Linux IaaS versleutelen. Schijf versleuteling combineert de standaard Windows BitLocker-functie en de Linux DM-cryptografie functie om volume versleuteling voor het besturings systeem en de gegevens schijven te bieden.

Azure Storage en Azure SQL Database gegevens op rest standaard versleutelen, en veel services bieden versleuteling als optie. U kunt Azure Key Vault gebruiken om controle te houden over de sleutels waarmee uw gegevens worden geopend en versleuteld. Zie [Azure resource providers Encryption model support voor meer informatie](encryption-atrest.md#azure-resource-providers-encryption-model-support).

**Aanbevolen procedures**: Versleuteling gebruiken om Risico's te verhelpen die betrekking hebben op onbevoegde toegang tot gegevens.   
**Details**: Versleutel uw stations voordat u er gevoelige gegevens naar schrijft.

Organisaties die geen gegevens versleuteling afdwingen, worden meer blootgesteld aan problemen met gegevens vertrouwelijkheid. Niet-geautoriseerde of Rogue gebruikers kunnen bijvoorbeeld gegevens stelen in gemanipuleerde accounts of ongeoorloofde toegang krijgen tot gegevens die zijn gecodeerd met een lees bare indeling. Bedrijven moeten er ook voor zorgen dat ze regel maat zijn en de juiste beveiligings maatregelen gebruiken om hun gegevens beveiliging te verbeteren teneinde te voldoen aan de industriële voor Schriften.

## <a name="protect-data-in-transit"></a>Gegevens in transit beveiligen

Het beveiligen van gegevens in transit moet een essentieel onderdeel zijn van uw strategie voor gegevens beveiliging. Omdat de gegevens naar een groot aantal locaties worden verplaatst, wordt u geadviseerd om altijd SSL/TLS-protocollen te gebruiken voor het uitwisselen van gegevens tussen verschillende locaties. In sommige gevallen wilt u mogelijk het volledige communicatie kanaal tussen uw on-premises en Cloud infrastructuren isoleren met behulp van een VPN.

Als u gegevens wilt verplaatsen tussen uw on-premises infra structuur en Azure, moet u rekening houden met de juiste veiligheids maatregelen zoals HTTPS of VPN. Gebruik [Azure VPN gateway](https://docs.microsoft.com/azure/vpn-gateway/)bij het verzenden van versleuteld verkeer tussen een virtueel Azure-netwerk en een on-premises locatie via het open bare Internet.

Hieronder vindt u aanbevolen procedures voor het gebruik van Azure VPN Gateway, SSL/TLS en HTTPS.

**Aanbevolen procedure**: Veilige toegang vanaf meerdere werk stations op locatie naar een virtueel Azure-netwerk.   
**Details**: [Site-naar-site-VPN](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)gebruiken.

**Aanbevolen procedure**: U kunt de toegang van een individueel werk station op locatie beveiligen met een virtueel Azure-netwerk.   
**Details**: Gebruik [punt-naar-site-VPN](/azure/vpn-gateway/vpn-gateway-point-to-site-create).

**Aanbevolen procedure**: Verplaats grotere gegevens sets over een speciale WAN-verbinding met hoge snelheid.   
**Details**: Gebruik [ExpressRoute](/azure/expressroute/expressroute-introduction). Als u ervoor kiest om ExpressRoute te gebruiken, kunt u de gegevens op toepassings niveau ook versleutelen met behulp van [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor extra beveiliging.

**Aanbevolen procedure**: Interactie met Azure Storage via de Azure Portal.   
**Details**: Alle trans acties worden uitgevoerd via HTTPS. U kunt ook [opslag rest API](https://msdn.microsoft.com/library/azure/dd179355.aspx) over https gebruiken om te communiceren met [Azure Storage](https://azure.microsoft.com/services/storage/).

Organisaties die gegevens in de overdracht niet kunnen [beveiligen, zijn](https://technet.microsoft.com/library/gg195641.aspx)gevoeliger voor [man-in-the-middle-aanvallen en het overnemen van](https://technet.microsoft.com/library/gg195821.aspx)sessies. Deze aanvallen kunnen de eerste stap zijn in het verkrijgen van toegang tot vertrouwelijke gegevens.

## <a name="secure-email-documents-and-sensitive-data"></a>Beveiligde e-mail, documenten en gevoelige gegevens

U wilt e-mail berichten, documenten en gevoelige gegevens die u deelt buiten uw bedrijf beheren en beveiligen. [Azure Information Protection](/azure/information-protection/) is een Cloud oplossing waarmee een organisatie documenten en e-mail berichten kan classificeren, labelen en beveiligen. Dit kan automatisch worden gedaan door beheerders die regels en voor waarden definiëren, hand matig door gebruikers of een combi natie van waar gebruikers aanbevelingen krijgen.

Classificatie kan te allen tijde worden geïdentificeerd, ongeacht waar de gegevens worden opgeslagen of met wie ze worden gedeeld. De labels bevatten visuele markeringen, zoals een koptekst, voet tekst of water merk. Meta gegevens worden toegevoegd aan bestanden en e-mail koppen in Lees bare tekst. De Lees bare tekst zorgt ervoor dat andere services, zoals oplossingen om gegevens verlies te voor komen, de classificatie kunnen identificeren en passende maat regelen te nemen.

De beveiligings technologie maakt gebruik van Azure Rights Management (Azure RMS). Deze technologie is geïntegreerd met andere micro soft-Cloud Services en-toepassingen, zoals Office 365 en Azure Active Directory. Deze beveiligings technologie maakt gebruik van versleuteling, identiteit en autorisatie beleid. Beveiliging die wordt toegepast via Azure RMS blijft met de documenten en e-mail berichten, onafhankelijk van de locatie, binnen of buiten uw organisatie, netwerken, bestands servers en toepassingen.

Met deze oplossing voor gegevens beveiliging blijft u de controle over uw gegevens, zelfs wanneer deze met andere personen worden gedeeld. U kunt Azure RMS ook gebruiken met uw eigen line-of-business-toepassingen en oplossingen voor gegevens beveiliging van software leveranciers, ongeacht of deze toepassingen en oplossingen on-premises of in de Cloud zijn.

U wordt aangeraden:

- [Implementeer Azure Information Protection](/azure/information-protection/deployment-roadmap) voor uw organisatie.
- Pas labels toe die overeenkomen met uw bedrijfs vereisten. Bijvoorbeeld: Pas een label toe met de naam ' zeer vertrouwelijk ' op alle documenten en e-mail berichten die gegevens van het hoogste geheim bevatten om deze gegevens te classificeren en te beveiligen. Vervolgens hebben alleen gemachtigde gebruikers toegang tot deze gegevens, met de beperkingen die u opgeeft.
- Configureer [gebruiks logboek registratie voor Azure RMS](/azure/information-protection/log-analyze-usage) zodat u kunt controleren hoe uw organisatie de beveiligings service gebruikt.

Organisaties die weinig [gegevens classificatie](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestands beveiliging hebben, zijn mogelijk gevoeliger voor gegevens lekkage of misbruik van gegevens. Met de juiste bestands beveiliging kunt u gegevens stromen analyseren om inzicht te krijgen in uw bedrijf, Risk ante gedrag te detecteren en corrigerende maat regelen te nemen, de toegang tot documenten te volgen, enzovoort.

## <a name="next-steps"></a>Volgende stappen

Zie [Aanbevolen procedures en patronen voor Azure-beveiliging](best-practices-and-patterns.md) voor meer aanbevolen procedures voor beveiliging bij het ontwerpen, implementeren en beheren van uw cloud oplossingen met behulp van Azure.

De volgende resources zijn beschikbaar om meer algemene informatie te geven over Azure-beveiliging en gerelateerde micro soft-Services:
* [Blog van het Azure-beveiligings team](https://blogs.msdn.microsoft.com/azuresecurity/) : voor actuele informatie over de nieuwste Azure-beveiliging
* [Micro soft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) : waar micro soft-beveiligings problemen, met inbegrip van problemen met Azure, kunnen worden gerapporteerd of via e-mail worden verzonden naarsecure@microsoft.com
