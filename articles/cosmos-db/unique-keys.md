---
title: Unieke sleutels in Azure Cosmos DB gebruiken
description: Informatie over het gebruik van unieke sleutels in uw Azure Cosmos-database
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.reviewer: sngun
ms.openlocfilehash: af3c7771ce977cf248c5f1b61ba1c535a10ccd3c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242511"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Beperkingen voor unieke sleutels in Azure Cosmos DB

Unieke sleutels toevoegen een laag van de integriteit van gegevens aan een Azure Cosmos-container. Bij het maken van een Azure Cosmos-container maakt u een unieke sleutel beleid. Met unieke sleutels ervoor u zorgen dat een of meer waarden in een logische partitie uniek is. Kunt u ook uniekheid per garanderen [partitiesleutel](partition-data.md). 

Nadat u een container met een unieke sleutel beleid maken, wordt het maken van een nieuwe of een update van een bestaand item leidt tot een duplicaat binnen een logische partitie voorkomen, zoals opgegeven door de unieke key-beperking. De partitiesleutel die in combinatie met de unieke sleutel zorgt ervoor dat de uniekheid van een item binnen het bereik van de container.

Neem bijvoorbeeld een Azure Cosmos-container met e-mailadres als de unieke key-beperking en `CompanyID` als de partitiesleutel. Wanneer u e-mailadres van de gebruiker met een unieke sleutel configureert, elk item heeft een uniek e-mailadres binnen een bepaalde `CompanyID`. Twee objecten kunnen niet worden gemaakt met dubbele e-mailadressen en de dezelfde waarde voor de partitiesleutel. 

Voor het maken van items met de dezelfde e-mailadres wordt meer paden in-adres, maar niet de dezelfde voornaam, achternaam en e-mailadres toevoegen aan het beleid voor unieke sleutels met beperkte toegang. In plaats van het maken van een unieke sleutel op basis van het e-mailadres, u kunt ook maken een unieke sleutel met een combinatie van de voornaam, achternaam en e-mailadres. Deze sleutel wordt ook wel een samengestelde unieke sleutel. In dit geval elke unieke combinatie van de drie waarden binnen een bepaalde `CompanyID` is toegestaan. 

De container kan bijvoorbeeld items met de volgende waarden in, waarbij elk item zich houdt aan de unieke key-beperking bevatten.

|CompanyID|Voornaam|Achternaam|E-mailadres|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Als u een ander item met de combinaties die worden vermeld in de vorige tabel invoegen probeert, ontvangt u een foutbericht. De fout geeft aan dat de unieke key-beperking niet is voldaan. U ontvangt een `Resource with specified ID or name already exists` of `Resource with specified ID, name, or unique index already exists` als een bericht geretourneerd. 

## <a name="define-a-unique-key"></a>Een unieke sleutel definiëren

Alleen wanneer u een Azure Cosmos-container maakt, kunt u unieke sleutels definiëren. Een unieke sleutel is afgestemd op een logische partitie. In het vorige voorbeeld, als u de container op basis van de postcode partitioneren uiteindelijk u dubbele items in elke logische partitie. Wanneer u unieke sleutels maakt, houd rekening met de volgende eigenschappen:

* U kunt een bestaande container voor het gebruik van een andere unieke sleutel niet bijwerken. Nadat een container is gemaakt met een unieke sleutel beleid, kunnen het beleid met andere woorden, kan niet worden gewijzigd.

* Om in te stellen een unieke sleutel voor een bestaande container, moet u een nieuwe container maken met de unieke key-beperking. Gebruik de juiste hulpprogramma voor gegevensmigratie te verplaatsen van de gegevens uit de bestaande container naar de nieuwe container. Voor SQL-containers, gebruikt u de [hulpprogramma voor gegevensmigratie](import-data.md) om gegevens te verplaatsen. Gebruik voor MongoDB-containers, [mongoimport.exe of mongorestore.exe](mongodb-migrate.md) om gegevens te verplaatsen.

* Een unieke sleutel beleid kan maximaal 16 pad waarden hebben. Bijvoorbeeld, de waarden zijn `/firstName`, `/lastName`, en `/address/zipCode`. Het beleid voor elke unieke sleutels kan maximaal 10 unique key-beperkingen of combinaties hebben. De gecombineerde paden voor elke beperking unique-index mag niet groter zijn dan 60 bytes. De voornaam, achternaam en e-mailadres, zijn in het vorige voorbeeld samen een beperking. Deze beperking maakt gebruik van 3 buiten de 16 mogelijke paden.

* Wanneer een container een unieke sleutel beleid heeft [aanvragen Aanvraageenheid (RU)](request-units.md) kosten in rekening gebracht voor het maken, bijwerken en verwijderen van een item iets hoger zijn.

* Sparse unieke sleutels worden niet ondersteund. Als er enkele waarden uniek pad ontbreken, moeten ze worden behandeld als null-waarden, die Neem deel aan de beperking voor uniekheid. Daarom kunnen er slechts één item met een null-waarde om te voldoen aan deze beperking.

* De unieke sleutelnamen zijn hoofdlettergevoelig. Neem bijvoorbeeld een container met de unieke key-beperking ingesteld op `/address/zipcode`. Als uw gegevens een veld met de naam `ZipCode`, Azure Cosmos DB invoegen 'null' als de unieke sleutel omdat `zipcode` is niet hetzelfde als `ZipCode`. Vanwege deze hoofdlettergevoeligheid kunnen niet alle records met ZipCode worden ingevoegd omdat het duplicaat 'null' is in strijd met de unieke key-beperking.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [logische partities](partition-data.md)
* Verken [unieke sleutels definiëren](how-to-define-unique-keys.md) bij het maken van een container
