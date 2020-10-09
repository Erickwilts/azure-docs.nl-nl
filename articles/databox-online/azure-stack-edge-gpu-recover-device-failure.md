---
title: Herstellen van een Azure Stack Edge Pro-apparaatfout
description: Hierin wordt beschreven hoe u een herstel bewerking kunt uitvoeren vanaf een Azure Stack Edge Pro-apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 10/06/2020
ms.author: alkohli
ms.openlocfilehash: ef99ff874376e0d76e45e4b0b2c36a78d938130b
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2020
ms.locfileid: "91843875"
---
# <a name="recover-from-a-failed-azure-stack-edge-pro-gpu-device"></a>Herstellen van een mislukt Azure Stack Edge Pro GPU-apparaat 

In dit artikel wordt beschreven hoe u een niet-toegestane fout kunt herstellen op uw Azure Stack Edge Pro GPU-apparaat. Een niet-toegestane fout op Azure Stack Edge Pro GPU-apparaat vereist een vervanging van het apparaat.

## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u over het volgende beschikt:

- Er wordt contact opgenomen met Microsoft Ondersteuning met betrekking tot het mislukken van het apparaat en er is een vervanging van het apparaat aanbevolen. 
- Maak een back-up van uw apparaatconfiguratie zoals beschreven in [voorbereiden op een apparaatfout](azure-stack-edge-gpu-prepare-device-failure.md).


## <a name="configure-replacement-device"></a>Vervangend apparaat configureren

Wanneer het apparaat een niet-toegestane fout tegen komt, moet u een vervangend apparaat best Ellen. De configuratie stappen voor het vervangende apparaat blijven hetzelfde. 

Haal de configuratie gegevens van het apparaat waarvan u een back-up hebt gemaakt van het apparaat dat is mislukt. U gebruikt deze informatie voor het configureren van het vervangende apparaat.  

Volg deze stappen om het vervangende apparaat te configureren:

1. Verzamel de vereiste informatie in de [implementatie controlelijst](azure-stack-edge-gpu-deploy-checklist.md). U moet de informatie gebruiken die u hebt opgeslagen in de vorige apparaatconfiguratie. 
1. Bestel een nieuw apparaat met dezelfde configuratie als de versie die is mislukt.  Als u een order wilt plaatsen, [maakt u een nieuwe Azure stack Edge-resource](azure-stack-edge-gpu-deploy-prep.md#) in de Azure Portal.
1. [Uw apparaat](azure-stack-edge-gpu-deploy-install.md#cable-the-device) [uitpakken](azure-stack-edge-gpu-deploy-install.md#unpack-the-device), op een [rek monteren](azure-stack-edge-gpu-deploy-install.md#rack-the-device) en aansluiten. 
1. [Verbinding maken met de lokale gebruikers interface van het apparaat](azure-stack-edge-gpu-deploy-connect.md).
1. Configureer het netwerk met behulp van de IP-adressen die u voor uw oude apparaat hebt gebruikt. Hierdoor wordt de impact op alle client computers in uw omgeving geminimaliseerd. Zie [netwerk instellingen configureren](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).
1. Wijs dezelfde apparaatnaam en hetzelfde DNS-domein als uw oude apparaat toe. Zo zorgt u ervoor dat uw clients dezelfde apparaatnaam kunnen gebruiken om met het nieuwe apparaat te praten. Zie [Apparaatinstellingen configureren](azure-stack-edge-gpu-deploy-set-up-device-update-time.md)voor meer informatie.
1. Configureer certificaten op het nieuwe apparaat op dezelfde manier als voor het oude apparaat. Houd er rekening mee dat het nieuwe apparaat een nieuw serie nummer voor het knoop punt heeft. Als u uw eigen certificaten op het oude apparaat hebt gebruikt, moet u een nieuw knooppunt certificaat ophalen. Zie [certificaten configureren](azure-stack-edge-gpu-deploy-configure-certificates.md)voor meer informatie.
1. Haal de activerings sleutel op uit het Azure Portal en activeer het nieuwe apparaat. Zie hoe u [het apparaat activeert](azure-stack-edge-gpu-deploy-activate.md).

U bent nu klaar om de werk belastingen te implementeren die u op het oude apparaat hebt uitgevoerd.

## <a name="restore-edge-cloud-shares"></a>Edge-Cloud shares herstellen

Volg deze stappen om de gegevens op de Edge-Cloud shares op uw apparaat te herstellen:

1. [Voeg shares toe](azure-stack-edge-j-series-manage-shares.md#add-a-share) met dezelfde share namen die eerder zijn gemaakt op het apparaat waarvoor een fout is opgetreden. Zorg ervoor dat bij het maken van shares de optie **BLOB-container** is ingesteld op gebruik van de **bestaande** opties en selecteer vervolgens de container die is gebruikt met het vorige apparaat.
1. [Gebruikers toevoegen](azure-stack-edge-j-series-manage-users.md#add-a-user) die toegang tot het vorige apparaat hadden.
1. [Voeg opslag accounts toe](azure-stack-edge-j-series-manage-storage-accounts.md#add-an-edge-storage-account) die zijn gekoppeld aan de shares eerder op het apparaat. Bij het maken van Edge-opslag accounts, selecteert u een bestaande container en wijst u de container aan die is toegewezen aan het Azure Storage account dat is toegewezen op het vorige apparaat. Alle gegevens van het apparaat die zijn geschreven naar het Edge-opslag account op het vorige apparaat, zijn geüpload naar de geselecteerde opslag container in het toegewezen Azure Storage-account.
1. [De share](azure-stack-edge-j-series-manage-shares.md#refresh-shares) gegevens van Azure vernieuwen. Hierdoor worden alle Cloud gegevens van de bestaande container naar de shares opgehaald.

## <a name="restore-edge-local-shares"></a>Edge-lokale shares herstellen

Als u een potentiële apparaatfout wilt voorbereiden, hebt u mogelijk een van de volgende back-upoplossingen geïmplementeerd om de lokale share gegevens van uw Kubernetes-of IoT-workloads te beveiligen:

| Software van derden           | Verwijzing naar de oplossing                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Neem contact op met Cohesity voor meer informatie.          |
| Veritas                        | Neem contact op met Veritas voor meer informatie.   |

Nadat het vervangende apparaat volledig is geconfigureerd, schakelt u het apparaat in voor lokale opslag. 

Voer de volgende stappen uit om de gegevens te herstellen vanaf lokale shares: 

1. [Configureer Compute op het apparaat](azure-stack-edge-gpu-deploy-configure-compute.md).
1. [Voeg een lokale share terug toe](azure-stack-edge-j-series-manage-shares.md#add-a-local-share) .
1. Voer de herstel procedure uit die is geboden door de gegevens beveiligings oplossing van de keuze. Raadpleeg de verwijzingen in de voor gaande tabel.

## <a name="restore-vm-files-and-folders"></a>VM-bestanden en-mappen herstellen

Als u een potentiële apparaatfout wilt voorbereiden, hebt u mogelijk een van de volgende back-upoplossingen geïmplementeerd om de gegevens op Vm's te beschermen:



| Back-upoplossingen        | Ondersteund besturings systeem   | Naslaginformatie                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| Microsoft Azure Recovery Services-agent (MARS) voor Azure Backup | Windows        | [Informatie over de MARS-agent](/azure/backup/backup-azure-about-mars)    |
| Cohesity                | Windows, Linux | [Korte Microsoft Azure-integratie, oplossing voor back-up en herstel](https://www.cohesity.com/solution/cloud/azure) <br>Neem contact op met Cohesity voor meer informatie.                          |
| Veritas                 | Windows, Linux | Neem contact op met Veritas voor meer informatie.                    |

Nadat het vervangende apparaat volledig is geconfigureerd, kunt u de virtuele machines opnieuw implementeren met de VM-installatie kopie die eerder is gebruikt. 

Voer de volgende stappen uit om de gegevens in de Vm's te herstellen:
 
1. [Een VM implementeren vanaf een VM-installatie kopie](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) op het apparaat. 
1. Installeer de keuze van de oplossing voor gegevens beveiliging op de VM.
1. Voer de herstel procedure uit die is geboden door de gegevens beveiligings oplossing van de keuze. Raadpleeg de verwijzingen in de voor gaande tabel.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [retour neren van een Azure stack Edge Pro-apparaat](azure-stack-edge-return-device.md).
