---
title: Overzicht van Azure Key Vault - Azure Key Vault | Microsoft Docs
description: Azure Key Vault is een cloudservice die werkt als een beveiligd geheimenarchief.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: overview
ms.custom: mvc
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: 456f828f09a710009d30e1c0a669cde658318ae0
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65748156"
---
# <a name="what-is-azure-key-vault"></a>Wat is Azure Sleutelkluis?

Met Azure Key Vault kunt u de volgende problemen oplossen:

- **Geheimenbeheer** - Met Azure Key Vault kunt u veilig de toegang tot tokens, wachtwoorden, certificaten, API-sleutels en andere geheimen opslaan en strikt beheren
- **Sleutelbeheer** - U kunt Azure Key Vault ook gebruiken als een oplossing voor sleutelbeheer. Met Azure Key Vault kunt u eenvoudig de versleutelingssleutels maken en beheren waarmee uw gegevens worden versleuteld. 
- **Certificaatbeheer** - Azure Key Vault is ook een service waarmee u eenvoudig openbare en persoonlijke SSL/TLS-certificaten (Secure Sockets Layer/Transport Layer Security) kunt inrichten, beheren en implementeren voor gebruik met Azure en uw interne verbonden bronnen. 
- **Sla geheimen op met hardwarebeveiligingsmodules (HSM's)** - De geheimen en sleutels kunnen worden beveiligd door software of met FIPS 140-2 niveau 2-validatie-HSM's

## <a name="why-use-azure-key-vault"></a>Waarom zou ik Azure Key Vault gebruiken?

### <a name="centralize-application-secrets"></a>Toepassingsgeheimen centraliseren

Door de opslag van toepassingsgeheimen te centraliseren in Azure Key Vault kunt u de verdeling ervan bepalen. Key Vault vermindert de kans dat geheimen per ongeluk worden gelekt aanzienlijk. Bij gebruik van Key Vault hoeven ontwikkelaars van toepassingen geen beveiligingsinformatie meer in de toepassing zelf op te slaan. Doordat u geen beveiligingsinformatie in toepassingen hoeft op te slaan elimineert u de noodzaak om deze informatie onderdeel van de code te maken. Een toepassing wil bijvoorbeeld verbinding maken met een database. In plaats van de verbindingsreeks op te slaan in de code van de app, kunt u deze veilig bewaren in Key Vault.

Met behulp van URI's hebben uw toepassingen veilig toegang tot de benodigde gegevens. Met deze URI's kunnen de toepassingen specifieke versies van een geheim ophalen. U hoeft geen aangepaste code te schrijven om de geheime informatie die in Key Vault is opgeslagen te beveiligen.

### <a name="securely-store-secrets-and-keys"></a>Geheimen en sleutels veilig opslaan

Geheimen en sleutels worden beveiligd door Azure. Hiervoor wordt gebruikgemaakt van algoritmen, sleutellengten en HSM's (Hardware Security Modules) die voldoen aan de industriestandaard. De gebruikte HSM's zijn Federal Information Processing Standards (FIPS) 140-2 Level 2 gevalideerd.

Voor toegang tot een sleutelkluis is de juiste verificatie en autorisatie vereist voordat een aanroeper (gebruiker of toepassing) toegang kan krijgen. Met verificatie wordt de identiteit van de aanroeper vastgesteld, terwijl autorisatie bepaalt welke bewerkingen de aanroeper mag uitvoeren.

Verificatie wordt uitgevoerd via Azure Active Directory. Autorisatie kan worden uitgevoerd via op rollen gebaseerd toegangsbeheer (RBAC) of Key Vault-toegangsbeleid. RBAC wordt gebruikt bij het beheren van de kluizen. Toegangsbeleid tot sleutelkluizen wordt gebruikt bij pogingen om toegang te krijgen tot gegevens in een kluis.

Sleutelkluizen van Azure kunnen software- of hardware-HSM beveiligd zijn. Voor situaties waar extra zekerheid is vereist, kunt u sleutels in HSM's (Hardware Security Modules) importeren of genereren die nooit verdergaan dan de HSM-grens. Microsoft gebruikt nCipher hardware security modules. U kunt nCipher-hulpprogramma's gebruiken om te verplaatsen van een sleutel van uw HSM naar Azure Key Vault.

Tot slot is Azure Key Vault zodanig ontworpen dat Microsoft uw gegevens niet kan zien of extraheren.

### <a name="monitor-access-and-use"></a>Toegang tot en gebruik van controles

Nadat u enkele sleutelkluizen hebt gemaakt, kunt u controleren hoe en wanneer er toegang tot uw sleutels en geheimen is gezocht. U kunt de activiteit controleren door logboekregistratie voor uw kluizen in te schakelen. U kunt Azure Key Vault voor het volgende configureren:

- Archiveren naar een opslagaccount.
- Streamen naar een Event Hub.
- De logboeken verzenden naar Azure Monitor-Logboeken.

U hebt de controle over uw logboeken en kunt ze beveiligen door de toegang te beperken. Bovendien kunt u logboeken verwijderen die u niet meer nodig hebt.

### <a name="simplified-administration-of-application-secrets"></a>Vereenvoudigd beheer van toepassingsgeheimen

Bij het opslaan van waardevolle gegevens moet u verschillende stappen uitvoeren. Beveiligingsinformatie moet worden beschermd, moet een bepaalde levenscyclus volgen en moet maximaal beschikbaar zijn. Met Azure Key Vault vereenvoudigt u het proces om aan deze vereisten te voldoen door:

- Het wegnemen van de noodzaak tot interne kennis van Hardware Security Modules
- Het op korte termijn omhoog schalen om de gebruikspieken van uw organisatie aan te kunnen.
- Het repliceren van de inhoud van uw Key Vault binnen een regio en naar een secundaire regio. Gegevensreplicatie zorgt voor een maximale beschikbaarheid en de beheerder hoeft geen actie te ondernemen om de failover te activeren.
- Het bieden van standaard Azure-beheeropties via de portal, Azure CLI en PowerShell.
- Het automatiseren van bepaalde taken voor certificaten die u aanschaft bij openbare CA's, zoals registreren en verlengen.

Bovendien kunt u met sleutelkluizen van Azure toepassingsgeheimen van elkaar scheiden. Toepassingen krijgen alleen toegang tot de kluis die ze mogen benaderen en ze mogen alleen specifieke bewerkingen uitvoeren. U kunt een Azure-sleutelkluis per toepassing maken en de geheimen die in een bepaalde sluitelkleus zijn opgeslagen beperken tot een specifieke toepassing en team van ontwikkelaars.

### <a name="integrate-with-other-azure-services"></a>Integreren met andere Azure-services

Key Vault wordt in Azure gebruikt als beveiligd archief om scenario's te vereenvoudigen, zoals:
-  [Azure Disk Encryption](../security/azure-security-disk-encryption.md)
-  De functionaliteit [altijd versleuteld]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) in SQL-server en Azure SQL Database
- [Azure App Service]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site). 

Key Vault zelf kan worden geïntegreerd met opslagaccounts, Event Hubs en logboekanalyses.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: een Azure Key Vault maken met behulp van de CLI](quick-create-cli.md)
- [Een Azure-webtoepassing configureren zodat deze een geheim vanuit een sleutelkluis kan lezen](tutorial-web-application-keyvault.md)
