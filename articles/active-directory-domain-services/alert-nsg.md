---
title: 'Azure Active Directory Domain Services: Configuratie voor het oplossen van Network Security Group | Microsoft Docs'
description: Oplossen van problemen met NSG-configuratie voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: mstephen
ms.openlocfilehash: 743f83fd25ff897492fda7103d3db1f4b961714d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246567"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Ongeldige configuratie van netwerken voor uw beheerde domein oplossen
Dit artikel helpt u problemen op te lossen netwerkgerelateerde configuratiefouten die in het volgende bericht weergegeven resulteren:

## <a name="alert-aadds104-network-error"></a>Waarschuwing AADDS104: Netwerkfout
**Waarschuwing:** *Microsoft kan niet tot de domeincontrollers voor dit beheerde domein. Dit kan gebeuren als een netwerkbeveiligingsgroep (NSG) geconfigureerd op uw virtuele netwerk blokkeert de toegang tot het beheerde domein. Een andere mogelijke reden is als er een door de gebruiker gedefinieerde route die het inkomende verkeer vanaf internet blokkeert.*

Ongeldige NSG-configuraties zijn de meest voorkomende oorzaak van netwerkfouten voor Azure AD Domain Services. De Netwerkbeveiligingsgroep (NSG) is geconfigureerd voor het virtuele netwerk toegang tot geven moet [specifieke poorten](network-considerations.md#ports-required-for-azure-ad-domain-services). Als deze poorten worden geblokkeerd, kan Microsoft controleren of bijwerken van uw beheerde domein. Bovendien wordt de synchronisatie tussen uw Azure AD-directory en uw beheerde domein beïnvloed. Tijdens het maken van uw NSG, laat u deze poorten open zijn om te voorkomen dat wordt onderbroken.

### <a name="checking-your-nsg-for-compliance"></a>Uw NSG op naleving controleren

1. Navigeer naar de [Netwerkbeveiligingsgroepen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) pagina in de Azure portal
2. Kies uit de tabel de NSG die is gekoppeld aan het subnet waarin uw beheerde domein is ingeschakeld.
3. Onder **instellingen** in het linkerdeelvenster klikt u op **inkomende beveiligingsregels**
4. Controleer de regels op locatie en bepalen welke regels worden blokkeert de toegang tot [deze poorten](network-considerations.md#ports-required-for-azure-ad-domain-services)
5. De NSG om te voldoen door de regel wordt verwijderd, een regel toe te voegen of het maken van een nieuwe NSG volledig bewerken. Stappen om [toevoegen van een regel](#add-a-rule-to-a-network-security-group-using-the-azure-portal) of maak een nieuwe, voldoen aan het beleid NSG hieronder

## <a name="sample-nsg"></a>Sample NSG
De volgende tabel ziet u een voorbeeld van een Netwerkbeveiligingsgroep die uw beheerde domein beveiligen houdt terwijl Microsoft om te controleren, beheren en bijwerken van gegevens.

![sample NSG](./media/active-directory-domain-services-alerts/default-nsg.png)

>[!NOTE]
> Azure AD Domain Services vereist onbeperkte uitgaande toegang van het virtuele netwerk. We raden aan geen te maken van een extra NSG-regel waarmee uitgaande toegang voor het virtuele netwerk wordt beperkt.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Een regel toevoegen aan een Netwerkbeveiligingsgroep met behulp van de Azure portal
Als u niet gebruiken van PowerShell wilt, kunt u handmatig enkele regels toevoegen aan de nsg's met behulp van de Azure portal. Voor het maken van regels in uw netwerkbeveiligingsgroep, voert u de volgende stappen uit:

1. Navigeer naar de [Netwerkbeveiligingsgroepen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) pagina in de Azure portal.
2. Kies uit de tabel de NSG die is gekoppeld aan het subnet waarin uw beheerde domein is ingeschakeld.
3. Onder **instellingen** in het linkerdeelvenster, klik op een **inkomende beveiligingsregels** of **uitgaande beveiligingsregels**.
4. Maak de regel door te klikken op **toevoegen** en de gegevens in te vullen. Klik op **OK**.
5. Controleer of dat de regel is gemaakt door te zoeken in de regeltabel.


## <a name="need-help"></a>Hulp nodig?
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](contact-us.md).
