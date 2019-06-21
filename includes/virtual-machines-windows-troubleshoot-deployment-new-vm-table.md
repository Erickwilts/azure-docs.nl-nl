---
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 14abae6f6f72d724fffb1ccaa12f56fb6976f7a1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176035"
---
De volgende tabel bevat de mogelijke uploaden en vastleggen combinaties van Windows (veld) gegeneraliseerde en gespecialiseerde (spec.) Installatiekopieën van het besturingssysteem. De combinaties die worden verwerkt zonder fouten worden aangeduid met een Y en die fouten genereert worden aangeduid met een N. De oorzaken en oplossingen voor de verschillende fouten die u wilt uitvoeren in zijn in de tabel hieronder.

| OS | Specificatie uploaden. | Alg uploaden. | Specificatie vastleggen. | Alg vastleggen. |
| --- | --- | --- | --- | --- |
| Windows-generatie. |N<sup>1</sup> |J |N<sup>3</sup> |J |
| Windows-specificatie. |J |N<sup>2</sup> |J |N<sup>4</sup> |

