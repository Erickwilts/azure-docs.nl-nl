---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: beb08c29587e4ce522131142fd61925b5af45fa9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66114570"
---
Wijzigingen in de Azure-bestandsshare met behulp van de Azure portal of SMB worden niet direct gedetecteerd en gerepliceerd, zoals wijzigingen in het servereindpunt. Azure Files heeft nog geen meldingen voor wachtwoordwijzigingen of logboekregistratie, dus er is geen manier voor het automatisch starten van een synchronisatiesessie wanneer bestanden worden gewijzigd. Op Windows Server, Azure File Sync gebruikt [Windows USN-logboek](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx) automatisch een om synchronisatiesessie te starten wanneer bestanden worden gewijzigd.<br /><br /> Voor het detecteren van wijzigingen in de Azure-bestandsshare, Azure File Sync een geplande taak met de naam is een *detectie taak wijzigen*. Een taak van de detectie van wijziging inventariseren van alle bestanden in de bestandsshare en vergelijkt deze naar de synchronisatieversie voor dat bestand. Wanneer de taak wijzigen-detectie wordt vastgesteld dat er bestanden zijn gewijzigd, wordt een synchronisatiesessie gestart in Azure File Sync. De wijziging detectie-taak wordt gestart om de 24 uur. Omdat deze wijziging detectie werkt met het inventariseren van alle bestanden in de Azure-bestandsshare, is de detectie van de wijziging duurt langer in grotere naamruimten dan in kleinere naamruimten. Voor grote naamruimten duurt het langer dan elke 24 uur om te bepalen welke bestanden zijn gewijzigd.<br /><br />
Opmerking, wijzigingen aangebracht in een Azure-bestandsshare met behulp van REST komt niet voor update van die de SMB tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie. <br /><br />
We zijn toe te voegen wijziging detectie voor een Azure-bestandsshare die vergelijkbaar is met USN voor volumes verkennen op Windows Server. Help ons prioriteren van deze functie voor de ontwikkeling van toekomstige stem voor via [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).
