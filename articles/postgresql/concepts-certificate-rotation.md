---
title: De draaiing van het certificaat voor Azure Database for PostgreSQL één server
description: Meer informatie over de aanstaande wijzigingen van basis certificaat wijzigingen die van invloed zijn op Azure Database for PostgreSQL één server
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 96720e156963a5fb542e72823a602aa2cc6a0ead
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93348998"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-postgresql-single-server"></a>Informatie over de wijzigingen in de basis-CA-wijziging voor Azure Database for PostgreSQL één server

Azure Database for PostgreSQL het basis certificaat voor de client toepassing/het stuur programma dat is ingeschakeld met SSL wijzigt, wordt gebruikt om [verbinding te maken met de database server](concepts-connectivity-architecture.md). Het basis certificaat dat momenteel beschikbaar is, is ingesteld op 15 februari 2021 (02/15/2021) als onderdeel van de aanbevolen procedures voor standaard onderhoud en beveiliging. In dit artikel vindt u meer informatie over de aanstaande wijzigingen, de bronnen die worden beïnvloed en de stappen die nodig zijn om ervoor te zorgen dat uw toepassing verbinding met uw database server onderhoudt.

>[!NOTE]
> Op basis van de feedback van klanten hebben we de afschaffing van het basis certificaat uitgebreid voor onze bestaande Baltimore-basis certificerings instantie van oktober 26, 2020 tot en met 15 februari 2021. We hopen dat deze uitbrei ding voldoende lever tijd biedt voor onze gebruikers om de client wijzigingen te implementeren als ze worden beïnvloed.

## <a name="what-update-is-going-to-happen"></a>Wat gebeurt er met de update?

In sommige gevallen gebruiken toepassingen een lokaal certificaat bestand dat is gegenereerd op basis van een certificaat bestand van een vertrouwde certificerings instantie (CA) om veilig verbinding te maken. Momenteel kunnen klanten alleen het vooraf gedefinieerde certificaat gebruiken om verbinding te maken met een Azure Database for PostgreSQL-server, die [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)zich bevindt. Het [browser forum van Certificate Authority (CA)](https://cabforum.org/) heeft echter onlangs rapporten gepubliceerd van meerdere certificaten die zijn uitgegeven door leveranciers van de certificerings instantie om niet-compatibel te zijn.

Conform de nalevings vereisten van de branche begon de leveranciers van de certificerings instantie CA-certificaten voor niet-compatibele Ca's te intrekken, waardoor het voor servers vereist dat certificaten worden gebruikt die zijn uitgegeven door compatibele Ca's en ondertekend zijn door CA-certificaten van die compatibele certificerings instanties. Omdat Azure Database for PostgreSQL momenteel een van deze niet-compatibele certificaten gebruikt, die client toepassingen gebruiken om hun SSL-verbindingen te valideren, moeten we ervoor zorgen dat de juiste acties worden ondernomen (hieronder beschreven) om de potentiële impact op uw PostgreSQL-servers te minimaliseren.

Het nieuwe certificaat wordt vanaf 15 februari 2021 (02/15/2021) gebruikt. Als u een CA-validatie of volledige validatie van het server certificaat gebruikt wanneer u verbinding maakt vanaf een PostgreSQL-client (sslmode = verifiëren-CA of sslmode = check-Full), moet u de configuratie van de toepassing bijwerken vóór 15 februari 2021 (02/15/2021).

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Hoe kan ik weet of mijn Data Base wordt beïnvloed?

Alle toepassingen die gebruikmaken van SSL/TLS en controleren of het basis certificaat het basis certificaat moet bijwerken. U kunt bepalen of uw verbindingen het basis certificaat verifiëren door uw connection string te controleren.
-    Als uw connection string bevat `sslmode=verify-ca` of `sslmode=verify-full` , moet u het certificaat bijwerken.
-    Als uw Connection String bevat `sslmode=disable` , `sslmode=allow` , `sslmode=prefer` , of `sslmode=require` , hoeft u geen certificaten bij te werken. 
-    Als uw connection string geen sslmode opgeeft, hoeft u geen certificaten bij te werken.

Als u een-client gebruikt die het connection string verwijderd, raadpleegt u de documentatie van de client om te begrijpen of de certificaten worden geverifieerd.

Zie de beschrijvingen van de [SSL-modus](https://www.postgresql.org/docs/11/libpq-ssl.html#ssl-mode-descriptions) in de postgresql-documentatie voor meer informatie over postgresql sslmode.

Als u wilt voor komen dat de beschik baarheid van uw toepassing wordt onderbroken omdat de certificaten onverwacht zijn ingetrokken, of als u een certificaat wilt bijwerken dat is ingetrokken, raadpleegt u de sectie [**' wat heb ik nodig om de verbinding te behouden '**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) .

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Wat heb ik nodig om de connectiviteit in stand te houden

Volg de onderstaande stappen om te voor komen dat de beschik baarheid van uw toepassing wordt onderbroken omdat de certificaten onverwacht zijn ingetrokken of als u een certificaat wilt bijwerken dat is ingetrokken. Het is een goed idee om een nieuw *. pem* -bestand te maken, waarin het huidige certificaat en de nieuwe en tijdens de validatie van het SSL-certificaat worden gecombineerd, zodra de toegestane waarden worden gebruikt. Raadpleeg de volgende stappen:

*   Down load BaltimoreCyberTrustRoot & DigiCertGlobalRootG2-basis certificerings instantie via onderstaande koppelingen:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Genereer een gecombineerd CA-certificaat archief met zowel **BaltimoreCyberTrustRoot** als **DigiCertGlobalRootG2** -certificaten.
    *   Voor Java-gebruikers (PostgreSQL JDBC) die gebruikmaken van DefaultJavaSSLFactory, voert u de volgende handelingen uit:

          ```console
          keytool -importcert -alias PostgreSQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```console
          keytool -importcert -alias PostgreSQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Vervang vervolgens het bestand van de oorspronkelijke opslag plaats door de nieuwe, gegenereerde versie:
        *   System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file"); 
        *   System. setProperty ("javax. net. SSL. trustStorePassword", "wacht woord");
        
    *   Voor .NET (Npgsql)-gebruikers in Windows, moet u ervoor zorgen dat **Baltimore Cyber Trust Root** en **DigiCert Global root G2** beide aanwezig zijn in Windows-certificaat archief, vertrouwde basis certificerings instanties. Als er geen certificaten bestaan, importeert u het ontbrekende certificaat.

        ![Azure Database for PostgreSQL .net-certificaat](media/overview/netconnecter-cert.png)

    *   Voor .NET (Npgsql)-gebruikers op Linux met SSL_CERT_DIR, moet u ervoor zorgen dat **BaltimoreCyberTrustRoot** en **DigiCertGlobalRootG2** beide bestaan in de map die wordt aangegeven door SSL_CERT_DIR. Als er geen certificaten bestaan, maakt u het ontbrekende certificaat bestand.

    *   Voor andere PostgreSQL-client gebruikers kunt u twee CA-certificaat bestanden als onderstaande indeling samen voegen

        </br>-----BEGIN CERTIFICAAT-----
 </br>(Root CA1: BaltimoreCyberTrustRoot. CRT. pem)
 </br>-----EIND CERTIFICAAT-----
 </br>-----BEGIN CERTIFICAAT-----
 </br>(Root CA2: DigiCertGlobalRootG2. CRT. pem)
 </br>-----EIND CERTIFICAAT-----

*   Vervang het oorspronkelijke PEM-bestand van de basis-CA door het gecombineerde basis-CA-bestand en start de toepassing/client opnieuw.
*    Nadat het nieuwe certificaat op de server is geïmplementeerd, kunt u in de toekomst het PEM-bestand van de CA wijzigen in DigiCertGlobalRootG2. CRT. pem.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Wat kan de invloed zijn van het bijwerken van het certificaat?
Als u het basis certificaat Baltimore Cyber Trust gebruikt om de SSL-verbinding met Azure Database for PostgreSQL te controleren, zoals hier wordt beschreven, kan de beschik baarheid van uw toepassing worden onderbroken omdat de data base niet bereikbaar is. Afhankelijk van uw toepassing kunnen er diverse fout berichten worden weer gegeven, inclusief, maar niet beperkt tot:
*    Ongeldig certificaat/ingetrokken certificaat
*    Time-out opgetreden voor verbinding

> [!NOTE]
> Verwijder het **Baltimore-certificaat** niet en pas het pas toe nadat het certificaat is gewijzigd. Er wordt een communicatie verzonden zodra de wijziging is aangebracht, waarna deze de Baltimore-certificaat kan verwijderen. 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

###    <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. als ik SSL/TLS niet gebruik, moet ik dan nog steeds de basis-CA bijwerken?
Er zijn geen acties vereist als u SSL/TLS niet gebruikt. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. als ik SSL/TLS gebruik, moet ik mijn database server opnieuw starten om de basis-CA bij te werken?
Nee, u hoeft de database server niet opnieuw op te starten om het nieuwe certificaat te gebruiken. Dit is een wijziging aan de client zijde en de binnenkomende client verbindingen moeten het nieuwe certificaat gebruiken om ervoor te zorgen dat ze verbinding kunnen maken met de database server.

### <a name="3-what-will-happen-if-i-do-not-update-the-root-certificate-before-february-15-2021-02152021"></a>3. Wat gebeurt er als ik het basis certificaat niet bijwerk vóór 15 februari 2021 (02/15/2021)?
Als u het basis certificaat niet bijwerkt vóór 15 februari 2021 (02/15/2021), kunnen uw toepassingen die verbinding maken via SSL/TLS en de verificatie voor het basis certificaat niet kunnen communiceren met de PostgreSQL-database server en de toepassing verbindings problemen ondervinden met uw PostgreSQL-database server.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-postgresql"></a>4. Wat is de impact van het gebruik van App Service met Azure Database for PostgreSQL?
Voor Azure app Services, het maken van verbinding met Azure Database for PostgreSQL, kunnen we twee mogelijke scenario's hebben. Dit is afhankelijk van hoe u SSL met uw toepassing gebruikt.
*   Dit nieuwe certificaat is toegevoegd aan App Service op platform niveau. Als u de SSL-certificaten gebruikt die zijn opgenomen in App Service platform in uw toepassing, is er geen actie vereist.
*   Als u expliciet het pad naar het SSL-certificaat bestand in uw code opneemt, moet u het nieuwe certificaat downloaden en de code bijwerken om het nieuwe certificaat te gebruiken. Een goed voor beeld van dit scenario is wanneer u aangepaste containers in App Service gebruikt zoals gedeeld in de [app service documentatie](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress)

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-postgresql"></a>5. Wat is de impact van het gebruik van Azure Kubernetes Services (AKS) met Azure Database for PostgreSQL?
Als u probeert verbinding te maken met de Azure Database for PostgreSQL met behulp van Azure Kubernetes Services (AKS), is dit vergelijkbaar met de toegang vanuit een specifieke host-omgeving van klanten. Raadpleeg de stappen [hier](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-postgresql"></a>6. Wat is de impact van Azure Data Factory om verbinding te maken met Azure Database for PostgreSQL?
Voor connector met Azure Integration Runtime gebruikt de connector certificaten in het Windows-certificaat archief in de op Azure gehoste omgeving. Deze certificaten zijn al compatibel met de zojuist toegepaste certificaten en daarom is er geen actie vereist.

Voor connectors die gebruikmaken van zelf-hostende Integration Runtime waarbij u het pad naar het SSL-certificaat bestand in uw connection string expliciet opneemt, moet u het [nieuwe certificaat](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) downloaden en de Connection String bijwerken om het te gebruiken.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. moet ik de downtime van een database server onderhoud voor deze wijziging plannen?
Nee. Omdat de wijziging hier alleen aan de client zijde wordt weer gegeven om verbinding te maken met de database server, is er geen uitval tijd nodig voor de database server voor deze wijziging.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-february-15-2021-02152021"></a>8. Wat moet ik doen als ik vóór 15 februari 2021 (02/15/2021) geen geplande downtime voor deze wijziging krijg?
Omdat de clients die zijn gebruikt voor het maken van een verbinding met de server, de certificaat gegevens moeten bijwerken zoals beschreven in de sectie oplossing [hier](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity), hoeven we in dit geval geen downtime voor de server te gebruiken.

### <a name="9-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>9. als ik een nieuwe server Maak na 15 februari 2021 (02/15/2021), wordt dit van invloed?
Voor servers die zijn gemaakt na 15 februari 2021 (02/15/2021), kunt u het zojuist uitgegeven certificaat voor uw toepassingen gebruiken om verbinding te maken via SSL.

###    <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. hoe vaak werkt micro soft hun certificaten bij of wat is het verloop beleid?
Deze certificaten die worden gebruikt door Azure Database for PostgreSQL worden door vertrouwde certificerings instanties (CA) verschaft. Daarom is de ondersteuning van deze certificaten op Azure Database for PostgreSQL gekoppeld aan de ondersteuning van deze certificaten per CA. In dit geval kunnen er echter onvoorziene fouten voor komen in deze vooraf gedefinieerde certificaten, die op het eerst moeten worden opgelost.

###    <a name="11-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-the-primary-server-or-the-read-replicas"></a>11. als ik lees replica's gebruik, moet ik deze update alleen op de primaire server of de Lees replica's uitvoeren?
Omdat deze update een wijziging aan de client zijde is, moet u de wijzigingen voor deze clients ook Toep assen als de client de gegevens van de replica server heeft gelezen. 

### <a name="12-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>12. hebben we een query aan de server zijde om te controleren of SSL wordt gebruikt?
Raadpleeg [SSL-verificatie](concepts-ssl-connection-security.md#applications-that-require-certificate-verification-for-tls-connectivity)om te controleren of u SSL-verbinding gebruikt om verbinding te maken met de server.

### <a name="13-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>13. is er een actie vereist als ik DigiCertGlobalRootG2 al in mijn certificaat bestand heb?
Nee. Er is geen actie vereist als uw certificaat bestand al het **DigiCertGlobalRootG2** heeft.

### <a name="14-what-is-you-are-using-docker-image-of-pgbouncer-sidecar-provided-by-microsoft"></a>14. u gebruikt nu docker-installatie kopie van PgBouncer-zijspaners van micro soft?
[Hieronder vindt](https://hub.docker.com/_/microsoft-azure-oss-db-tools-pgbouncer-sidecar) u een nieuwe docker-installatie kopie die zowel [**Baltimore**](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) als [**DigiCert**](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) ondersteunt. U kunt deze nieuwe installatie kopie ophalen om onderbrekingen te voor komen in connectiviteit vanaf 15 februari 2021. 

###    <a name="15-what-if-i-have-further-questions"></a>15. Wat moet ik doen als ik meer vragen heb?
Als u vragen hebt, kunt u antwoorden krijgen van experts van community's in [micro soft Q&A](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com). Als u een ondersteunings abonnement hebt en technische hulp nodig hebt, kunt u [contact met ons](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com) opnemen
