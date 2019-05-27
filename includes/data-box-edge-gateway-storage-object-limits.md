---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161290"
---
Hier vindt u de grootte van de Azure-objecten die kunnen worden geschreven. Zorg ervoor dat alle bestanden die worden geüpload, aan deze limieten voldoen.

| Azure-objecttype | Uploadlimiet                                             |
|-------------------|-----------------------------------------------------------|
| Blok-blob        | ~ 4.75 TB                                                 |
| Pagina-blob         | 1 TB <br> Elk bestand dat is geüpload in de indeling van de pagina-Blob moet zijn uitgelijnd 512 bytes (een integraal meerdere), anders het uploaden is mislukt. <br> De VHD en VHDX zijn dan 512 bytes uitgelijnd. |
| Azure Files         | 1 TB <br> Elk bestand dat is geüpload in de indeling van de pagina-Blob moet zijn uitgelijnd 512 bytes (een integraal meerdere), anders het uploaden is mislukt. <br> De VHD en VHDX zijn dan 512 bytes uitgelijnd. |

> [!IMPORTANT]
> Het maken van bestanden (ongeacht het opslagtype) mag maximaal 5 TB. Echter, als u een bestand waarvan de grootte groter dan de uploadlimiet gedefinieerd in de voorgaande tabel is maakt, het bestand heeft geen ophalen geüpload. U moet handmatig verwijderen van het bestand om de vrij te maken.