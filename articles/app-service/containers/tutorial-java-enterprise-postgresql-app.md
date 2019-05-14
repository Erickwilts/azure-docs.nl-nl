---
title: Een Java Enterprise-web-app maken onder Linux - Azure App Service | Microsoft Docs
description: Leer hoe u een Java Enterprise-app kunt maken die in Wildfly in Azure App Service onder Linux kan worden uitgevoerd.
author: JasonFreeberg
manager: routlaw
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 11/13/2018
ms.author: jafreebe
ms.custom: seodec18
ms.openlocfilehash: 6b9c9500423392ec07482f049697d9b49dc060bf
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65603181"
---
# <a name="tutorial-build-a-java-ee-and-postgres-web-app-in-azure"></a>Zelfstudie: een Java EE- en Postgres-web-app in Azure maken

Deze zelfstudie leert u hoe u een web-app voor Java Enterprise Edition (EE) op Azure App Service maken en verbinden met een Postgres-database. Wanneer u klaar bent, hebt u een [WildFly](https://www.wildfly.org/about/)-toepassing die gegevens opslaat in [Azure Database for Postgres](https://azure.microsoft.com/services/postgresql/) en wordt uitgevoerd op [Azure App Service voor Linux](app-service-linux-intro.md).

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een Java EE-app in Azure implementeren via Maven
> * Een Postgres-database maken in Azure
> * De WildFly-server configureren om Postgres te gebruiken
> * De app bijwerken en opnieuw implementeren
> * Moduletests uitvoeren op WildFly

## <a name="prerequisites"></a>Vereisten

1. [Download en installeer Git](https://git-scm.com/)
2. [Download and install Maven 3](https://maven.apache.org/install.html) (Maven downloaden en installeren)
3. [Azure CLI downloaden en installeren](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="clone-and-edit-the-sample-app"></a>Voorbeeld-app klonen en bewerken

In deze stap gaat u de voorbeeldtoepassing klonen en het objectmodel van het Maven-project (POM of pom.xml) configureren voor de implementatie.

### <a name="clone-the-sample"></a>Het voorbeeld klonen

Navigeer naar een werkmap in het terminalvenster en kloon de [voorbeeldopslagplaats](https://github.com/Azure-Samples/wildfly-petstore-quickstart).

```bash
git clone https://github.com/Azure-Samples/wildfly-petstore-quickstart.git
```

### <a name="update-the-maven-pom"></a>Maven-POM bijwerken

Werk de Maven-invoegtoepassing van Azure bij met de gewenste naam en resourcegroep van uw App Service. U hoeft niet van tevoren het App Service-plan of -exemplaar te maken. Met de Maven-invoegtoepassing worden de resourcegroep en App Service gemaakt indien deze nog niet aanwezig zijn. 

U kunt naar beneden scrollen naar de sectie `<plugins>` van _pom.xml_, regel 200, om de wijzigingen te maken. 

```xml
<!-- Azure App Service Maven plugin for deployment -->
<plugin>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-webapp-maven-plugin</artifactId>
  <version>${version.maven.azure.plugin}</version>
  <configuration>
    <appName>YOUR_APP_NAME</appName>
    <resourceGroup>YOUR_RESOURCE_GROUP</resourceGroup>
    <linuxRuntime>wildfly 14-jre8</linuxRuntime>
  ...
</plugin>  
```
Vervang `YOUR_APP_NAME` en `YOUR_RESOURCE_GROUP` met de namen van uw App Service en de resourcegroep.

## <a name="build-and-deploy-the-application"></a>De toepassing compileren en implementeren

We gaan Maven gebruiken om de toepassing te maken en in App Service te implementeren.

### <a name="build-the-war-file"></a>Het WAR-bestand maken

De POM in dir project wordt geconfigureerd om de toepassing in te pakken in een webarchiefbestand (WAR). De toepassing maken met Maven:

```bash
mvn clean install -DskipTests
```

De testscenario's in deze toepassing zijn bedoeld om te worden uitgevoerd als de toepassing in WildFly wordt geïmplementeerd. De tests die lokaal worden gebouwd, worden overgeslagen. U voert de tests uit zodra de toepassing in App Service is geïmplementeerd.

### <a name="deploy-to-app-service"></a>Implementeren naar App Service

Nu het WAR-bestand klaar is, kan de Azure-invoegtoepassing worden gebruikt om het bestand in App Service te implementeren:

```bash
mvn azure-webapp:deploy
```

Als de implementatie klaar is, gaat u verder met de volgende stap.

### <a name="create-a-record"></a>Record maken

Open een browser en ga naar `https://<your_app_name>.azurewebsites.net/`. Gefeliciteerd, u hebt een Java EE-toepassing geïmplementeerd in Azure App Service.

De toepassing gebruikt nu een in-memory H2-database. Klik in de navigatiebalk op Beheerder en maak een nieuwe categorie. De record in uw in-memory database gaat verloren als het App Service-exemplaar opnieuw start. In de volgende stappen lost u dit op door een Postgres-database in Azure in te richten en WildFly te configureren om deze te gebruiken.

![Locatie knop Beheerder](media/tutorial-java-enterprise-postgresql-app/admin_button.JPG)

## <a name="provision-a-postgres-database"></a>Postgres-database inrichten

Als u een Postgres-databaseserver wilt inrichten, opent u een terminalvenster en voert u de volgende opdracht uit met de gewenste waarden voor de servernaam, de gebruikersnaam, het wachtwoord en de locatie. Gebruik de resourcegroep waarin zich uw App Service bevindt. Noteer uw wachtwoord voor later gebruik.

```bash
az postgres server create -n <desired-name> -g <same-resource-group> --sku-name GP_Gen4_2 -u <desired-username> -p <desired-password> -l <location>
```

Ga naar de portal en zoek uw Postgres-database. Als de blade actief is, kopieert u de waarden voor Servernaam en Aanmeldingsnaam van de serverbeheerder. Deze hebt u later nodig.

### <a name="allow-access-to-azure-services"></a>Toegang tot Azure-services toestaan

Zet in het paneel **Verbindingsbeveiliging** van de blade Azure-database de knop Toegang tot Azure-services toestaan in de positie **AAN**.

![Toegang tot Azure-services toestaan](media/tutorial-java-enterprise-postgresql-app/postgress_button.JPG)

## <a name="update-your-java-app-for-postgres"></a>De Java-app bijwerken voor Postgres

We gaan nu enkele wijzigingen aan de Java-toepassing aanbrengen, zodat deze gebruik kan maken van de Postgres-database.

### <a name="add-postgres-credentials-to-the-pom"></a>Referenties voor Postgres aan de POM toevoegen

Vervang in _pom.xml_ de hoofdletterwaarden in de tijdelijke aanduiding door de naam van de Postgres-server, de aanmeldingsnaam van de beheerder en het wachtwoord. Deze velden bevinden zich binnen de Maven-invoegtoepassing van Azure. (Vervang `YOUR_SERVER_NAME`, `YOUR_PG_USERNAME`, en `YOUR_PG_PASSWORD` in de `<value>`-tags... niet binnen de `<name>`-tags!)

```xml
<plugin>
      ...
      <appSettings>
      <property>
        <name>POSTGRES_CONNECTIONURL</name>
        <value>jdbc:postgresql://YOUR_SERVER_NAME:5432/postgres?ssl=true</value>
      </property>
      <property>
        <name>POSTGRES_USERNAME</name>
        <value>YOUR_PG_USERNAME</value>
      </property>
      <property>
        <name>POSTGRES_PASSWORD</name>
        <value>YOUR_PG_PASSWORD</value>
      </property>
    </appSettings>
  </configuration>
</plugin>
```

### <a name="update-the-java-transaction-api"></a>Java Transaction API bijwerken

Vervolgens moet de configuratie van de Java Transaction API (JTA) worden bewerkt, zodat de Java-toepassing kan communiceren met Postgres in plaats van de in-memory H2-database die we eerder hebben gebruikt. Open een editor voor _src/main/resources/META-INF/persistence.xml_. Vervang de waarde voor `<jta-data-source>` door `java:jboss/datasources/postgresDS`. De XML van de JTA moet nu de volgende instelling hebben:

```xml
...
<jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
...
```

## <a name="configure-the-wildfly-application-server"></a>WildFly-toepassingsserver configureren

Voordat de opnieuw geconfigureerde toepassing kan worden geïmplementeerd, moet de WildFly-toepassingsserver met de Postgres-module en de bijbehorende afhankelijkheden worden bijgewerkt. Meer configuratie-informatie kan worden gevonden op [WildFly configureren server](configure-language-java.md#configure-java-ee-wildfly).

Voor het configureren van de server hebben we de vier bestanden in de `wildfly_config/`-map nodig:

- **postgresql-42.2.5.jar**: dit JAR-bestand is het JDBC-stuurprogramma voor Postgres. Zie de [officiële website](https://jdbc.postgresql.org/index.html) (Engelstalig) voor meer informatie.
- **postgres-module.xml**: dit XML-bestand declareert een naam voor de Postgres-module (org.postgres). Het geeft teven de resources en afhankelijkheden op die nodig zijn voor de te gebruiken module.
- **jboss_cli_commands.cl**: dit bestand bevat configuratieopdrachten die door de JBoss CLI worden uitgevoerd. Met de opdrachten wordt de Postgres-module aan de WildFly-toepassingsserver toegevoegd, worden de referenties opgegeven, wordt een JNDI-naam gedeclareerd, de drempelwaarde voor de time-out ingesteld, enzovoort. Zie de [officiële documentatie](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli) (Engelstalig) als u niet bekend bent met de JBoss CLI.
- **startup_script.sh**: ten slotte wordt dit script uitgevoerd als uw App Service-exemplaar wordt gestart. Er wordt slechts één functie uitgevoerd: het doorsluizen van de opdrachten in `jboss_cli_commands.cli` naar de JBoss CLI.

U wordt ten sterkste aangeraden de inhoud van deze bestanden te lezen, met name _jboss_cli_commands.cli_.

### <a name="ftp-the-configuration-files"></a>Configuratiebestanden via FTP verzenden

De inhoud van `wildfly_config/` moet via FTP naar onze App Service-exemplaar worden verzonden. U kunt uw FTP-referenties ophalen door op de knop **Publicatieprofiel ophalen** te klikken in de App Service-blade in de Azure-portal. De gebruikersnaam en het wachtwoord voor de FTP staan in het gedownloade XML-document. Zie [dit document](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials) voor meer informatie over het publicatieprofiel.

U kunt de vier bestanden in `wildfly_config/` naar `/home/site/deployments/tools/` overdragen met een FTP-programma naar keuze. (Draag alleen de bestanden over, niet de hele map.)

### <a name="finalize-app-service"></a>App Service voltooien

Ga in de App Service-blade naar het paneel Toepassingsinstellingen. Stel onder 'Runtime' 'Opstartbestand' in op `/home/site/deployments/tools/startup_script.sh`. Dit garandeert dat het shellscript wordt uitgevoerd nadat het App Service-exemplaar is gemaakt, maar voordat de WildFly-server wordt gestart.

Start ten slotte App Service opnieuw. De knop bevindt zich in het paneel Overzicht.

## <a name="redeploy-the-application"></a>De toepassing opnieuw implementeren

U kunt de toepassing opnieuw samenstellen en implementeren in een terminalvenster.

```bash
mvn clean install -DskipTests azure-webapp:deploy
```

Gefeliciteerd! De toepassing maakt nu gebruik van een Postgres-database en eventuele records die in de toepassing worden gemaakt, worden in Postgres opgeslagen in plaats van in de eerder gebruikte in-memory H3-database. U kunt dit controleren door een record te maken en App Service opnieuw te starten. De records zijn nog steeds aanwezig als u de toepassing opnieuw start.

## <a name="clean-up"></a>Opruimen

Als u deze resources niet voor een andere zelfstudie nodig hebt (zie Volgende stappen), kunt u ze verwijderen door de volgende opdracht uit te voeren:

```bash
az group delete --name <your-resource-group>
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Java EE-app in Azure implementeren via Maven
> * Een Postgres-database maken in Azure
> * De WildFly-server configureren om Postgres te gebruiken
> * De app bijwerken en opnieuw implementeren
> * Moduletests uitvoeren op WildFly

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw app kunt toewijzen.

> [!div class="nextstepaction"]
> [Zelfstudie: Aangepaste DNS-naam toewijzen aan uw app](../app-service-web-tutorial-custom-domain.md)

Of Ga naar andere resources:

> [!div class="nextstepaction"]
> [Java-app configureren](configure-language-java.md)
