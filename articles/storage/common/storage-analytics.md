---
title: Azure Storage Analytics gebruiken voor het verzamelen van Logboeken en metrische gegevens | Microsoft Docs
description: Met Opslaganalyse kunt u metrische gegevens voor alle opslag services bijhouden en logboeken voor blob-, wachtrij-en tabel opslag verzamelen.
services: storage
author: normesta
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: e812bf03bf2adb852becf56954e7039aa3eb0633
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565909"
---
# <a name="storage-analytics"></a>Storage Analytics

Met Azure Storage Analytics wordt logboekregistratie uitgevoerd en kunnen metrische gegevens voor een opslagaccount worden geleverd. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount.

Als u Opslaganalyse wilt gebruiken, moet u deze afzonderlijk inschakelen voor elke service die u wilt bewaken. U kunt deze functie inschakelen via de [Azure Portal](https://portal.azure.com). Zie [een opslag account bewaken in de Azure Portal](storage-monitor-storage-account.md)voor meer informatie. U kunt Opslaganalyse ook via een programma inschakelen via de REST API of de client bibliotheek. Gebruik de [Eigenschappen van de set BLOB service](/rest/api/storageservices/set-blob-service-properties), [Stel Queue service](/rest/api/storageservices/set-queue-service-properties)-eigenschappen in, [Stel Table service-eigenschappen](/rest/api/storageservices/set-table-service-properties)in en [Stel bestands service-eigenschappen](/rest/api/storageservices/Get-File-Service-Properties) bewerkingen in om Opslaganalyse voor elke service in te scha kelen.

De geaggregeerde gegevens worden opgeslagen in een bekende BLOB (voor logboek registratie) en in bekende tabellen (voor metrieken), die mogelijk worden geopend met behulp van de Blob service-en Table service-Api's.

Opslaganalyse heeft een limiet van 20 TB voor de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw opslag account. Zie [Azure Storage schaal baarheid en prestatie doelen](storage-scalability-targets.md)voor meer informatie over limieten voor opslag accounts.

Zie [Microsoft Azure Storage controleren, diagnosticeren en problemen oplossen](storage-monitoring-diagnosing-troubleshooting.md)voor meer informatie over het gebruik van Opslaganalyse en andere hulpprogram ma's voor het identificeren, vaststellen en oplossen van problemen met Azure Storage.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="billing-for-storage-analytics"></a>Facturering voor Opslaganalyse

Alle metrische gegevens worden geschreven door de services van een opslag account. Als gevolg hiervan is elke schrijf bewerking die door Opslaganalyse is uitgevoerd Factureerbaar. Daarnaast is de hoeveelheid opslag ruimte die wordt gebruikt door de metrische gegevens ook Factureerbaar.

De volgende acties die door Opslaganalyse worden uitgevoerd, zijn Factureerbaar:

* Aanvragen voor het maken van blobs voor logboek registratie.
* Aanvragen voor het maken van tabel entiteiten voor metrische gegevens.

Als u een Bewaar beleid voor gegevens hebt geconfigureerd, worden er geen kosten in rekening gebracht voor verwijderings transacties wanneer Opslaganalyse oude logboeken en metrische gegevens verwijdert. Het verwijderen van trans acties van een client is echter Factureerbaar. Zie [een Opslaganalyse Bewaar beleid voor gegevens instellen](https://msdn.microsoft.com/library/azure/hh343263.aspx)voor meer informatie over het Bewaar beleid.

### <a name="understanding-billable-requests"></a>Informatie over factureer bare aanvragen

Elke aanvraag voor de opslag service van een account is Factureerbaar of niet factureerbaar. Opslaganalyse registreert elke afzonderlijke aanvraag voor een service, met inbegrip van een status bericht dat aangeeft hoe de aanvraag is verwerkt. Op dezelfde manier Opslaganalyse worden metrische gegevens opgeslagen voor zowel een service als de API-bewerkingen van die service, inclusief de percentages en het aantal bepaalde status berichten. Samen kunnen deze functies u helpen bij het analyseren van uw factureer bare aanvragen, het aanbrengen van verbeteringen in uw toepassing en het vaststellen van problemen met aanvragen voor uw services. Zie [Wat is Azure Storage facturering: band breedte, trans acties en capaciteit](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)voor meer informatie over facturering.

Wanneer u Opslaganalyse gegevens bekijkt, kunt u de tabellen in het onderwerp [Opslaganalyse vastgelegde bewerkingen en status berichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) gebruiken om te bepalen welke aanvragen Factureerbaar zijn. Vervolgens kunt u uw logboeken en metrische gegevens vergelijken met de status berichten om te zien of er een bepaalde aanvraag in rekening is gebracht. U kunt ook de tabellen in het vorige onderwerp gebruiken om de beschik baarheid voor een opslag service of afzonderlijke API-bewerking te onderzoeken.

## <a name="next-steps"></a>Volgende stappen
* [Een opslag account in de Azure Portal bewaken](storage-monitor-storage-account.md)
* [Opslaganalyse metrische gegevens](storage-analytics-metrics.md)
* [Opslaganalyse logboek registratie](storage-analytics-logging.md)
