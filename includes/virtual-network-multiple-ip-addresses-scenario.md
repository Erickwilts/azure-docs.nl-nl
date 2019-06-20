---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 3df4108907a4e1e65a444faf1049163966b7accf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175957"
---
## <a name="scenario"></a>Scenario
Een virtuele machine met één NIC is gemaakt en verbonden met een virtueel netwerk. De virtuele machine vereist drie verschillende *persoonlijke* IP-adressen en twee *openbare* IP-adressen. De IP-adressen zijn toegewezen aan de volgende IP-configuraties:

* **IP-configuratie-1:** Wijst een *statische* privé IP-adres en een *statische* openbaar IP-adres.
* **IPConfig-2:** Wijst een *statische* privé IP-adres en een *statische* openbaar IP-adres.
* **IPConfig-3:** Wijst een *statische* privé IP-adres en geen openbaar IP-adres.
  
    ![Meerdere IP-adressen](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

De IP-configuraties zijn gekoppeld aan de NIC wanneer de NIC is gemaakt en de NIC is gekoppeld aan de virtuele machine wanneer de virtuele machine wordt gemaakt. De typen van IP-adressen die worden gebruikt voor het scenario zijn bedoeld als voorbeeld. Welke IP-adres en de toewijzing typen dat u nodig hebt, kunt u toewijzen.

> [!NOTE]
> Hoewel de stappen in dit artikel worden alle IP-configuraties op één NIC toegewezen, kunt u ook meerdere IP-configuraties toewijzen aan een NIC in een multi-NIC-VM. Lees voor meer informatie over het maken van een virtuele machine met meerdere NIC's, de [een virtuele machine maken met meerdere NIC's](../articles/virtual-machines/windows/multiple-nics.md) artikel.