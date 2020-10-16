---
title: De Azure VMware Solution-jumpbox maken
description: Stappen om de Azure VMware Solution-jumpbox te maken.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 5d5a5d82cf6e70e7ad55bcfa10c7f85372131c87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91578411"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-access-private-cloud.md -->

1. Selecteer in de resourcegroep **+ Toevoegen**, zoek en selecteer **Microsoft Windows 10** en selecteer vervolgens **Maken**.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Een nieuwe virtuele Windows 10-machine toevoegen voor een jumpbox." border="true":::

1. Voer de vereiste gegevens in de velden in en selecteer **Controleren + maken**. 

   Raadpleeg de volgende tabel voor meer informatie over deze items.

   | Veld | Waarde |
   | --- | --- |
   | **Abonnement** | Waarde is automatisch ingevuld met het Abonnement dat bij de Resourcegroep hoort. |
   | **Resourcegroep** | De waarde is automatisch ingevuld voor de huidige Resourcegroep, die u in de vorige zelfstudie hebt gemaakt.  |
   | **Naam van virtuele machine** | Voer een unieke naam voor de VM in. |
   | **Regio** | Selecteer de geografische locatie van de virtuele machine. |
   | **Beschikbaarheidsopties** | Laat de standaardwaarde ingeschakeld. |
   | **Installatiekopie** | Selecteer de VM-installatiekopie. |
   | **Grootte** | Laat de waarde van de standaardgrootte staan. |
   | **Verificatietype**  | Selecteer **Wachtwoord**. |
   | **Gebruikersnaam** | Voer de gebruikersnaam in voor aanmelding bij de virtuele machine. |
   | **Wachtwoord** | Voer het wachtwoord in om u aan te melden bij de virtuele machine. |
   | **Wachtwoord bevestigen** | Voer het wachtwoord in om u aan te melden bij de virtuele machine. |
   | **Openbare binnenkomende poorten** | Selecteer **Geen**. Als u Geen selecteert, kunt u [JIT-toegang](../../security-center/security-center-just-in-time.md#jit-configure) gebruiken om de toegang tot de virtuele machine alleen te beheren wanneer u toegang wilt.  |


1. Wanneer de validatie is geslaagd, selecteert u **Maken** om het proces voor het maken van de virtuele machine te starten.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Een nieuwe virtuele Windows 10-machine toevoegen voor een jumpbox." border="true":::