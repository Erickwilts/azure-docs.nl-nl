---
title: De Azure MFA NPS-extensie configureren-Azure Active Directory
description: Nadat u de NPS-uitbrei ding hebt geïnstalleerd, gebruikt u deze stappen voor geavanceerde configuratie, zoals IP-white list en UPN-vervanging.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e156585ba063515bd8be573b5d99b41e7ce35d1
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70932490"
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Geavanceerde configuratie opties voor de NPS-extensie voor Multi-Factor Authentication

De Network Policy Server (NPS) uitbrei ding breidt Azure Multi-Factor Authentication-functies in de Cloud uit naar uw on-premises infra structuur. In dit artikel wordt ervan uitgegaan dat u de uitbrei ding al hebt geïnstalleerd en wilt weten hoe u de uitbrei ding voor uw behoeften kunt aanpassen. 

## <a name="alternate-login-id"></a>Alternatieve aanmeldings-ID

Aangezien de NPS-extensie verbinding maakt met zowel uw on-premises als in de Cloud, kan er een probleem optreden waarbij uw on-premises UPN (User Principal Names) niet overeenkomen met de namen in de Cloud. Gebruik alternatieve aanmeldings-Id's om dit probleem op te lossen. 

U kunt binnen de NPS-uitbrei ding een Active Directory kenmerk opgeven dat moet worden gebruikt in plaats van de UPN voor Azure Multi-Factor Authentication. Zo kunt u uw on-premises resources beveiligen met verificatie in twee stappen zonder uw lokale Upn's te wijzigen. 

Als u alternatieve aanmeldings-id's wilt configureren `HKLM\SOFTWARE\Microsoft\AzureMfa` , gaat u naar en bewerkt u de volgende register waarden:

| Name | type | Standaardwaarde | Description |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | string | Leeg | Geef de naam op van Active Directory kenmerk dat u wilt gebruiken in plaats van de UPN. Dit kenmerk wordt gebruikt als het kenmerk AlternateLoginId. Als deze register waarde is ingesteld op een [geldig Active Directory kenmerk](https://msdn.microsoft.com/library/ms675090.aspx) (bijvoorbeeld E-mail of DisplayName), wordt de waarde van het kenmerk gebruikt in plaats van de UPN van de gebruiker voor authenticatie. Als deze register waarde leeg of niet geconfigureerd is, wordt AlternateLoginId uitgeschakeld en wordt de UPN van de gebruiker voor verificatie gebruikt. |
| LDAP_FORCE_GLOBAL_CATALOG | boolean | False | Gebruik deze vlag om het gebruik van globale catalogus voor LDAP-Zoek opdrachten af te dwingen bij het opzoeken van AlternateLoginId. Configureer een domein controller als een globale catalogus, voeg het kenmerk AlternateLoginId toe aan de globale catalogus en schakel deze vlag in. <br><br> Als LDAP_LOOKUP_FORESTS is geconfigureerd (niet leeg), **wordt deze vlag afgedwongen als waar**, ongeacht de waarde van de register instelling. In dit geval vereist de NPS-uitbrei ding dat de globale catalogus moet worden geconfigureerd met het kenmerk AlternateLoginId voor elke forest. |
| LDAP_LOOKUP_FORESTS | string | Leeg | Geef een door punt komma's gescheiden lijst met forests op die u wilt doorzoeken. Bijvoorbeeld: *contoso. com; foobar. com*. Als deze register waarde is geconfigureerd, doorzoekt de NPS-extensie iteratief alle forests in de volg orde waarin ze werden vermeld en wordt de eerste geslaagde AlternateLoginId-waarde geretourneerd. Als deze register waarde niet is geconfigureerd, wordt de AlternateLoginId-zoek opdracht beperkt tot het huidige domein.|

Als u problemen met alternatieve aanmeldings-Id's wilt oplossen, gebruikt u de aanbevolen stappen voor [alternatieve aanmeldings-ID-fouten](howto-mfa-nps-extension-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP-uitzonde ringen

Als u de beschik baarheid van de server wilt bewaken, bijvoorbeeld als load balancers controleren welke servers worden uitgevoerd voordat workloads worden verzonden, wilt u deze controles niet blok keren door verificatie aanvragen. Maak in plaats daarvan een lijst met IP-adressen die u kent door service accounts, en schakel Multi-Factor Authentication vereisten voor die lijst uit.

Als u een lijst met toegestane IP-adressen `HKLM\SOFTWARE\Microsoft\AzureMfa` wilt configureren, gaat u naar en configureert u de volgende register waarde:

| Name | type | Standaardwaarde | Description |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | string | Leeg | Geef een door punt komma's gescheiden lijst met IP-adressen op. Voeg de IP-adressen van computers toe waar service aanvragen afkomstig zijn, zoals de NAS/VPN-server. IP-adresbereiken en subnetten worden niet ondersteund. <br><br> Bijvoorbeeld *10.0.0.1; 10.0.0.2; 10.0.0.3*.

> [!NOTE]
> Deze register sleutel wordt niet standaard gemaakt door het installatie programma en er wordt een fout weer gegeven in het AuthZOptCh-logboek wanneer de service opnieuw wordt gestart. Deze fout in het logboek kan worden genegeerd, maar als deze register sleutel is gemaakt en leeg blijft als deze niet nodig is, wordt het fout bericht niet weer gegeven.

Wanneer een aanvraag afkomstig is van een IP-adres dat in de `IP_WHITELIST`is, wordt verificatie in twee stappen overgeslagen. De IP-lijst wordt vergeleken met het IP-adres dat is opgenomen in het kenmerk *ratNASIPAddress* van de RADIUS-aanvraag. Als een RADIUS-aanvraag binnenkomt zonder het kenmerk ratNASIPAddress, wordt de volgende waarschuwing vastgelegd: "P_WHITE_LIST_WARNING:: IP-white list wordt genegeerd omdat het bron-IP-adres ontbreekt in de RADIUS-aanvraag in het kenmerk NasIpAddress."

## <a name="next-steps"></a>Volgende stappen

[Resolve error messages from the NPS extension for Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md) (Foutberichten van de NPS-extensie voor Azure Multi-Factor Authentication oplossen)
