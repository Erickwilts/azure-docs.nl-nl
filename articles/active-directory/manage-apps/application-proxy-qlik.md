---
title: Azure AD-toepassingsproxy en Qlik Sense | Microsoft Docs
description: Toepassingsproxy inschakelen in Azure portal en installeer de Connectors voor de omgekeerde proxy.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/06/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f54e08e6c3b7b673541f124a90f32dbc860fa44
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65859542"
---
# <a name="application-proxy-and-qlik-sense"></a>De toepassingsproxy en Qlik Sense 
Azure Active Directory-toepassingsproxy en Qlik Sense zijn een partnerschap aangegaan samen om te controleren of dat u zich eenvoudig kunt toepassingsproxy gebruiken voor externe toegang voor uw implementatie Qlik Sense.  

## <a name="prerequisites"></a>Vereisten 
De rest van dit scenario wordt ervan uitgegaan dat u het volgende gedaan:
 
- Geconfigureerd [Qlik Sense](https://community.qlik.com/docs/DOC-19822). 
- [Een Application Proxy-connector hebt geïnstalleerd](application-proxy-add-on-premises-application.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Uw toepassingen publiceren in Azure 
Als u wilt publiceren QlikSense, moet u twee toepassingen publiceren in Azure.  

### <a name="application-1"></a>Toepassing #1: 
Volg deze stappen voor het publiceren van uw app. Voor een meer gedetailleerd overzicht van de stappen 1-8, Zie [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-add-on-premises-application.md). 


1. Meld u aan de Azure-portal als globale beheerder. 
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen**. 
3. Selecteer **toevoegen** aan de bovenkant van de blade. 
4. Selecteer **On-premises toepassing**. 
5. Vul de vereiste velden met informatie over de nieuwe app. Gebruik de volgende richtlijnen voor de instellingen: 
   - **Interne URL**: Deze toepassing moet een interne URL de URL is QlikSense zelf hebben. Bijvoorbeeld, **https&#58;//demo.qlikemm.com:4244** 
   - **Methode voor verificatie vooraf**: Azure Active Directory (aanbevolen maar niet vereist) 
1. Selecteer **toevoegen** aan de onderkant van de blade. Uw toepassing wordt toegevoegd en het menu Snel starten wordt geopend. 
2. Selecteer in het menu snelle start **een gebruiker toewijzen voor het testen van**, en ten minste één gebruiker toevoegen aan de toepassing. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing. 
3. Selecteer **toewijzen** om op te slaan van de toewijzing van de gebruiker testen. 
4. (Optioneel) Selecteer op de blade app-beheer van eenmalige aanmelding. Kies **beperkte Kerberos-delegering** uit de vervolgkeuzelijst en vul de vereiste velden in op basis van uw configuratie Qlik. Selecteer **Opslaan**. 

### <a name="application-2"></a>Toepassing #2: 
Volg dezelfde stappen als voor de toepassing #1, met de volgende uitzonderingen: 

**Stap 5 van #**: Nu moet de interne URL de URL van de QlikSense met de verificatiepoort die wordt gebruikt door de toepassing. De standaardwaarde is **4244** voor HTTPS en 4248 voor HTTP. Bijvoorbeeld: **https&#58;//demo.qlik.com:4244**</br></br> 
**Stap 10 #:** Geen een SSO instellen, en laat de **eenmalige aanmelding uitgeschakeld**
 
 
## <a name="testing"></a>Testen 
Uw toepassing is nu gereed om te testen. Toegang tot de externe URL die u gebruikt voor het publiceren van QlikSense in #-toepassing 1 en meld u aan als een gebruiker die is toegewezen aan beide toepassingen.  

## <a name="additional-references"></a>Aanvullende naslaginformatie
Raadpleeg de artikelen van de Community Qlik te volgen voor meer informatie over publiceren Qlik Sense met Application Proxy: 
- [Azure AD met geïntegreerde Windows-verificatie met behulp van een Kerberos-beperkte overdracht met Qlik Sense](https://community.qlik.com/docs/DOC-20183)
- [Integratie met Azure AD-toepassingsproxy Qlik Sense](https://community.qlik.com/t5/Technology-Partners-Ecosystem/Azure-AD-Application-Proxy/ta-p/1528396)

## <a name="next-steps"></a>Volgende stappen

- [Publiceren van toepassingen met toepassingsproxy](application-proxy-add-on-premises-application.md)
- [Werken met Application Proxy connectors](application-proxy-connector-groups.md)

