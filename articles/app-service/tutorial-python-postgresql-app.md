---
title: 'Zelfstudie: Een Python Django-app met Postgres implementeren'
description: Een Python-web-app maken met een PostgreSQL-database en deze implementeren naar Azure. Deze zelfstudie gebruikt het Django-framework en de app wordt gehost op Azure App Service op Linux.
ms.devlang: python
ms.topic: tutorial
ms.date: 10/09/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
- devx-track-python
- devx-track-azurecli
ms.openlocfilehash: 63fdee6036580df42f7f965244b5f888c1ec082d
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540751"
---
# <a name="tutorial-deploy-a-django-web-app-with-postgresql-in-azure-app-service"></a>Zelfstudie: Een Django-web-app implementeren met PostgreSQL in Azure App Service

In deze zelfstudie ziet u hoe u een gegevensgestuurde web-app van Python [Django](https://www.djangoproject.com/) naar [Azure App Service](overview.md) implementeert en deze koppelt aan een Azure Database for Postgres-database. App Service biedt een uiterst schaalbare webhostingservice met self-patchfunctie.

In deze zelfstudie gebruikt u de Azure CLI om de volgende taken te voltooien:

> [!div class="checklist"]
> * Uw initiële omgeving instellen met Python en de Azure CLI
> * Een Azure Database for PostgreSQL-database maken
> * Code implementeren naar Azure App Service en koppelen aan PostgreSQL
> * Uw code bijwerken en opnieuw implementeren
> * Diagnostische logboeken weergeven
> * De web-app in Azure Portal beheren

U kunt ook de [versie voor de Azure-portal van deze zelfstudie](/azure/developer/python/tutorial-python-postgresql-app-portal) gebruiken.


## <a name="set-up-your-initial-environment"></a>Uw eerste omgeving instellen

1. U moet beschikken over een Azure-account met een actief abonnement. [Gratis een account maken](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
1. Installeer <a href="https://www.python.org/downloads/" target="_blank">Python 3.6 of hoger</a>.
1. Installeer de <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.0.80 of hoger, waarmee u opdrachten kunt uitvoeren in elke willekeurige shell voor het inrichten en configureren van Azure-resources.

Open een terminalvenster en controleer of uw Python-versie 3.6 of hoger is:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 --version
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```cmd
py -3 --version
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 --version
```

---

Controleer of uw Azure CLI-versie 2.0.80 of hoger is:

```azurecli
az --version
```

Meld u vervolgens aan bij Azure via de CLI:

```azurecli
az login
```

Met deze opdracht wordt een browser geopend om uw referenties te verzamelen. Wanneer de opdracht is voltooid, wordt JSON-uitvoer weergegeven met informatie over uw abonnementen.

Zodra u bent aangemeld, kunt u Azure-opdrachten uitvoeren met de Azure CLI om te werken met resources in uw abonnement.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

## <a name="clone-or-download-the-sample-app"></a>De voorbeeld-app klonen of downloaden

# <a name="git-clone"></a>[Git-kloon](#tab/clone)

Kloon de voorbeeldopslagplaats:

```terminal
git clone https://github.com/Azure-Samples/djangoapp
```

Open vervolgens die map:

```terminal
cd djangoapp
```

# <a name="download"></a>[Downloaden](#tab/download)

Ga naar [https://github.com/Azure-Samples/djangoapp](https://github.com/Azure-Samples/djangoapp), selecteer **Kloon** en vervolgens **ZIP-bestand downloaden**. 

Pak het ZIP-bestand uit in een map met de naam *djangoapp*. 

Open vervolgens een terminalvenster in die map *djangoapp*.

---

Het voorbeeld van de djangoapp bevat de gegevensgestuurde Django-polls-app die u kunt verkrijgen door [Uw eerste Django-app schrijven](https://docs.djangoproject.com/en/3.1/intro/tutorial01/) in de Django-documentatie te volgen. De voltooide app wordt hier voor het gemak aangeboden.

Het voorbeeld is ook aangepast om te worden uitgevoerd in een productie-omgeving, zoals App Service:

- Productie-instellingen bevinden zich in het bestand *azuresite/production.py*. De ontwikkelingsgegevens bevinden zich in *azuresite/settings.py*.
- De app gebruikt productie-instellingen wanneer de `DJANGO_ENV`-omgevingsvariabele is ingesteld op 'productie'. Verderop in de zelfstudie maakt u deze omgevingsvariabele samen met andere die worden gebruikt voor de configuratie van de PostgreSQL-database.

Deze wijzigingen dienen specifiek om Django uit te voeren in een productieomgeving en zijn niet uniek voor App Service. Zie de sectie [Controlelijst voor Django-implementatie](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/) voor meer informatie. Zie ook [Productie-instellingen voor Django op Azure](configure-language-python.md#production-settings-for-django-apps) voor meer informatie over een aantal van de wijzigingen.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

## <a name="create-postgres-database-in-azure"></a>Een Postgres-database maken in Azure

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](../postgresql/concepts-pricing-tiers.md#compute-generations-and-vcores) is available in your region. -->

De `db-up`-extensie installeren voor de Azure CLI:

```azurecli
az extension add --name db-up
```

Als de `az`-opdracht niet wordt herkend, controleert u of de Azure CLI is geïnstalleerd volgens de beschrijving in [Uw initiële omgeving instellen](#set-up-your-initial-environment).

Maak vervolgens de Postgres-database in Azure met de opdracht [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up):

```azurecli
az postgres up --resource-group DjangoPostgres-tutorial-rg --location westus2 --sku-name B_Gen5_1 --server-name <postgres-server-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

- Vervang *\<postgres-server-name>* door een naam die overal in Azure uniek is (het servereindpunt wordt `https://<postgres-server-name>.postgres.database.azure.com`). Het is handig om een combinatie van uw bedrijfsnaam en een andere unieke waarde te gebruiken.
- Geef voor *\<admin-username>* en *\<admin-password>* referenties op om een gebruiker met beheerdersrechten te maken voor deze Postgres-server. Gebruik het `$`-teken niet in de gebruikersnaam of het wachtwoord. Later maakt u omgevingsvariabelen met deze waarden, waarbij het `$`-teken een speciale betekenis heeft in de Linux-container die wordt gebruikt om Python-apps uit te voeren.
- De [prijscategorie](../postgresql/concepts-pricing-tiers.md) B_Gen5_1 (Basic, Gen5, 1 kern) die hier wordt gebruikt, is de voordeligste. Laat voor productiedatabases het argument `--sku-name` weg om de prijscategorie GP_Gen5_2 (Algemeen gebruik, Gen 5, 2 kernen) te gebruiken.

Deze opdracht voert de volgende acties uit, dit kan enkele minuten duren:

- Er wordt een [resourcegroep](../azure-resource-manager/management/overview.md#terminology) gemaakt met de naam `DjangoPostgres-tutorial-rg`, als deze nog niet bestaat.
- Maak een Postgres-server die een naam krijgt door het argument `--server-name`.
- Maak een beheerdersaccount met behulp van de argumenten `--admin-user` en `--admin-password`. U kunt deze argumenten weglaten om ervoor te zorgen dat de opdracht unieke referenties voor u genereert.
- Maak een `pollsdb`-database zoals benoemd door het argument `--database-name`.
- Er wordt toegang verleend vanaf uw lokale IP-adres.
- Er wordt toegang verleend vanuit Azure-services.
- Maak een databasegebruiker met toegang tot de `pollsdb`-database.

U kunt alle stappen afzonderlijk uitvoeren met `az postgres`- en `psql`-opdrachten, maar `az postgres up` combineert alle stappen.

Wanneer de opdracht voltooid is, wordt een JSON-object uitgevoerd dat verschillende verbindingsreeksen voor de database bevat, samen met de URL van de server, een gegenereerde gebruikersnaam (zoals 'joyfulKoala@msdocs-djangodb-12345') en een GUID-wachtwoord. Kopieer de korte gebruikersnaam (voor de @) en het wachtwoord naar een tijdelijk bestand. U hebt deze verderop in deze zelfstudie nodig.

<!-- not all locations support az postgres up -->
> [!TIP]
> `-l <location-name>` kan worden ingesteld op een van de [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/). U kunt de regio's beschikbaar maken voor uw abonnement met de opdracht [`az account list-locations`](/cli/azure/account#az-account-list-locations). Voor productie-apps plaatst u uw database en uw app in dezelfde locatie.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

## <a name="deploy-the-code-to-azure-app-service"></a>De code implementeren naar Azure App Service

In deze sectie maakt u een app-host in de App Service-app, koppelt u deze app aan de Postgres-database en implementeert u vervolgens uw code naar die host.

### <a name="create-the-app-service-app"></a>De App Service-app maken

Controleer in de terminal of u zich in de opslagmap *djangoapp* die de code van de app bevat bevindt.

Een App Service-app maken (het hostproces) met de opdracht [`az webapp up`](/cli/azure/webapp#az-webapp-up):

```azurecli
az webapp up --resource-group DjangoPostgres-tutorial-rg --location westus2 --plan DjangoPostgres-tutorial-plan --sku B1 --name <app-name>
```
<!-- without --sku creates PremiumV2 plan -->

- Gebruik voor het argument `--location` dezelfde locatie als voor de database in het vorige onderdeel.
- Vervang *\<app-name>* door een naam die overal in Azure uniek is (het servereindpunt is `https://<app-name>.azurewebsites.net`). De tekens die voor *\<app-name>* zijn toegestaan, zijn `A`-`Z`, `0`-`9` en `-`. Het is handig om een een combinatie van uw bedrijfsnaam en een app-id te gebruiken.

Deze opdracht voert de volgende acties uit, dit kan enkele minuten duren:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Maak de [resourcegroep](../azure-resource-manager/management/overview.md#terminology) als deze nog niet bestaat. (In deze opdracht gebruikt u dezelfde resourcegroep waarin u de database eerder hebt gemaakt.)
- Maak het [App Service-abonnement](overview-hosting-plans.md) *DjangoPostgres-tutorial-plan* in de Basic-prijscategorie (B1), als deze nog niet bestaat. `--plan` en `--sku` zijn optioneel.
- Maak de App Service-app als deze nog niet bestaat.
- Schakel standaardlogboeken voor de app in, als die nog niet zijn ingeschakeld.
- Upload de opslagplaats met behulp van ZIP-implementatie, met ingeschakelde bouwautomatisering.
- Cache dezelfde parameters, zoals de naam van de resourcegroep en het App Service-abonnement, in het bestand *.azure/config*. Bijgevolg hoeft u niet al diezelfde parameters op te geven met latere opdrachten. Om bijvoorbeeld de app opnieuw te implementeren na wijzigingen, kunt u `az webapp up` gewoon opnieuw uitvoeren zonder parameters. Opdrachten die afkomstig zijn van CLI-extensies, zoals `az postgres up`, gebruiken de cache echter niet. Daarom moet u hier de resourcegroep en locatie opgeven met bij het eerste gebruik van `az webapp up`.

Na een geslaagde implementatie genereert de opdracht JSON-uitvoer, zoals in het volgende voorbeeld:

![Voorbeelduitvoer van de opdracht az webapp up](./media/tutorial-python-postgresql-app/az-webapp-up-output.png)

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

> [!NOTE]
> Als u de URL van de app op dit moment probeert te bezoeken, dan krijgt u het foutbericht 'DisallowedHost op /'. Deze fout doet zich voor omdat u de app nog niet heeft geconfigureerd om de eerder besproken productie-instellingen te gebruiken. Dit gaat u doen in het volgende onderdeel.

### <a name="configure-environment-variables-to-connect-the-database"></a>De omgevingsvariabelen configureren om de database te verbinden

Nu de code is geïmplementeerd naar App Service, is de volgende stap om de app te verbinden met de Postgres-database in Azure.

De code van de app verwacht database-informatie te vinden in vier omgevingsvariabelen genaamd `DBHOST`, `DBNAME`, `DBUSER` en `DBPASS`. Als u productie-instellingen wilt gebruiken, moet u ook de omgevingsvariabele `DJANGO_ENV` instellen op `production`.

Om omgevingsvariabelen in te stellen in App Service, maakt u 'app-instellingen' met de volgende opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set).

```azurecli
az webapp config appsettings set --settings DJANGO_ENV="production" DBHOST="<postgres-server-name>" DBNAME="pollsdb" DBUSER="<username>" DBPASS="<password>"
```

- Vervang *\<postgres-server-name>* door de naam die u eerder hebt gebruikt met de opdracht `az postgres up`. De code in *azuresite/production.py* voegt automatisch `.postgres.database.azure.com` toe om de volledige Postgres-server-URL te maken.
- Vervang *\<username>* en *\<password>* door de beheerdersreferenties die u hebt gebruikt in combinatie met de eerdere `az postgres up`-opdracht of die `az postgres up` voor u heeft gegenereerd. De code in *azuresite/production.py* bouwt automatisch de volledige Postgres-gebruikersnaam op van `DBUSER` en `DBHOST`. Dus neem het gedeelte `@server` niet op. (Zoals eerder opgemerkt, moet u het `$`-teken in geen van beide waarden gebruiken omdat het een speciale betekenis heeft voor Linux-omgevingsvariabelen.)
- De resourcegroep en de namen van de app worden opgehaald uit de cachewaarden in het bestand *.azure/config*.

In uw Python-code opent u deze instellingen als omgevingsvariabelen met instructies zoals `os.environ.get('DJANGO_ENV')`. Zie [Omgevingsvariabelen openen](configure-language-python.md#access-environment-variables) voor meer informatie.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

### <a name="run-django-database-migrations"></a>Django-databasemigraties uitvoeren

Django-databasemigraties zorgen ervoor dat het schema in de PostgreSQL van de Azure-database overeenkomt met de schema's die in uw code beschreven worden.

1. Open een SSH-sessie in de browser door naar de volgende URL te gaan en u aan te melden met de referenties van uw Azure-account (niet de referenties van de databaseserver).

    ```
    https://<app-name>.scm.azurewebsites.net/webssh/host
    ```

    Vervang `<app-name>` door de naam die u eerder hebt gebruikt in de opdracht `az webapp up`.

    U kunt in macOS en Linux ook verbinding maken met een SSH-sessie met de opdracht [`az webapp ssh`](/cli/azure/webapp?view=azure-cli-latest&preserve-view=true#az_webapp_ssh).

    Als u geen verbinding kunt maken met de SSH-sessie, kan de app zelf niet worden gestart. [Raadpleeg de diagnostische logboeken](#stream-diagnostic-logs) voor meer informatie. Als u de benodigde app-instellingen in de vorige sectie bijvoorbeeld niet hebt gemaakt, wordt in de logboeken `KeyError: 'DBNAME'` aangegeven.

1. Voer in de SSH-sessie de volgende opdrachten uit (u kunt opdrachten plakken met **CTRL**+**Shift**+**V** ):

    ```bash
    # Change to the folder where the app code is deployed
    cd site/wwwroot
    
    # Activate default virtual environment in App Service container
    source /antenv/bin/activate

    # Install packages
    pip install -r requirements.txt

    # Run database migrations
    python manage.py migrate

    # Create the super user (follow prompts)
    python manage.py createsuperuser
    ```

1. De `createsuperuser`-opdracht vraagt u om de referenties van de super gebruiker op te geven. Gebruik voor deze zelfstudie de standaard gebruikersnaam `root`, druk op **Enter** voor het e-mailadres om het leeg te laten en voer `Pollsdb1` in bij het wachtwoord.

1. Als er een foutmelding wordt weer geven dat de database is vergrendeld, zorg er dan voor dat u de `az webapp settings`-opdracht hebt uitgevoerd in de vorige sectie. Zonder deze instellingen kan de migratie-opdracht niet communiceren met de database, wat resulteert in de fout.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)
    
### <a name="create-a-poll-question-in-the-app"></a>Een poll-vraag maken in de app

1. Open de URL `http://<app-name>.azurewebsites.net` in een browser. De app zou het bericht 'Er zijn geen polls beschikbaar' moeten weergeven, omdat er nog geen specifieke polls in de database zitten.

    Als u 'Toepassingsfout' ziet, is het waarschijnlijk dat u de vereiste instellingen niet hebt gemaakt in de vorige stap, [Omgevingsvariabelen configureren om verbinding te maken met de database](#configure-environment-variables-to-connect-the-database), of dat deze waarde fouten bevat. Voer de opdracht `az webapp config appsettings list` uit om de instellingen te controleren. U kunt ook [de diagnostische logboeken controleren](#stream-diagnostic-logs) om specifieke fouten te bekijken tijdens het opstarten van de app. Als u de instellingen bijvoorbeeld niet hebt gemaakt, wordt de fout `KeyError: 'DBNAME'` weergegeven in de logboeken.

    Nadat u de instellingen hebt bijgewerkt om eventuele fouten te corrigeren, geeft u de app een minuut om opnieuw op te starten en vernieuwt u vervolgens de browser.

1. Blader naar `http://<app-name>.azurewebsites.net/admin`. Meld u aan met de beheerdersreferenties uit het vorige onderdeel (`root` en `Pollsdb1`). Selecteer onder **Polls** **Toevoegen** naast **Vragen** en maak een poll-vraag met enkele antwoordmogelijkheden.

1. Ga opnieuw naar `http://<app-name>.azurewebsites.net` om te controleren of de gebruiker nu de vragen te zien krijgt. Beantwoord vragen zoals u wilt om wat gegevens de genereren in de database.

**Gefeliciteerd!** U voert een Python (Django) web-app uit in Azure App Service voor Linux met een actieve Postgres-database.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

> [!NOTE]
> In App Service wordt een Django-project gedetecteerd door in elke submap naar een *wsgi.py* -bestand te zoeken dat standaard wordt gemaakt met `manage.py startproject`. Wanneer App Service dat bestand heeft gevonden, wordt de Django web-app geladen. Zie [Ingebouwde Python-installatiekopie configureren](configure-language-python.md) voor meer informatie.

## <a name="make-code-changes-and-redeploy"></a>Wijzigingen aanbrengen in code en opnieuw implementeren

In dit onderdeel maakt u lokale wijzigingen in de app en implementeert u de code opnieuw naar App Service. Tijdens dat proces stelt u een virtuele Python-omgeving in die ondersteuning biedt bij doorlopend werk.

### <a name="run-the-app-locally"></a>De app lokaal uitvoeren

Voer in een terminalvenster de volgende opdrachten uit. Volg de aanwijzingen wanneer u de supergebruiker aanmaakt:

# <a name="bash"></a>[bash](#tab/bash)

```bash
# Configure the Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# Configure the Python virtual environment
py -3 -m venv venv
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
venv\scripts\activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="cmd"></a>[CMD](#tab/cmd)

```cmd
:: Configure the Python virtual environment
py -3 -m venv venv
venv\scripts\activate

:: Install packages
pip install -r requirements.txt
:: Run Django migrations
python manage.py migrate
:: Create Django superuser (follow prompts)
python manage.py createsuperuser
:: Run the dev server
python manage.py runserver
```
---

Zodra de web-app volledig geladen is, voorziet de ontwikkelingsserver voor Django de URL van de lokale app in het bericht 'Ontwikkelingsserver starten op http://127.0.0.1:8000/. Verlaat de server met CTRL-BREAK'.

![Voorbeelduitvoer van Django-ontwikkelingsserver](./media/tutorial-python-postgresql-app/django-dev-server-output.png)

Test de app lokaal met de volgende stappen:

1. Ga in een browser naar `http://localhost:8000`, waar het bericht 'Geen polls beschikbaar' zou moeten worden weergegeven. 

1. Ga naar `http:///localhost:8000/admin` en meld u aan als de gebruiker met beheerdersrechten die u voorheen hebt gemaakt. Selecteer onder **Polls** opnieuw **Toevoegen** naast **Vragen** en maak een poll-vraag met enkele antwoordmogelijkheden. 

1. Ga weer naar *http:\//localhost:8000* en beantwoord de vraag om de app te testen. 

1. Stop de Django-server door op **CTRL**+**C** te drukken.

Wanneer de app lokaal wordt uitgevoerd, wordt er een lokale Sqlite3-database gebruikt en wordt uw productiedatabase niet verstoord. U kunt, indien gewenst, ook een lokale PostgreSQL-database gebruiken om uw productieomgeving beter te simuleren.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

### <a name="update-the-app"></a>De app bijwerken

Zoek in `polls/models.py` naar de regel die begint met `choice_text` en verander de parameter `max_length` naar 100:

```python
# Find this lie of code and set max_length to 100 instead of 200
choice_text = models.CharField(max_length=100)
```

Omdat u het gegevensmodel heeft gewijzigd, moet u een nieuwe Django-migratie maken en de database migreren:

```
python manage.py makemigrations
python manage.py migrate
```

Voer de ontwikkelingsserver opnieuw uit met `python manage.py runserver` en test de app op *http:\//localhost: 8000/admin* :

Stop de Django-webserver opnieuw met **CTRL**+**C**.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

### <a name="redeploy-the-code-to-azure"></a>De code opnieuw implementeren naar Azure

Voer vanuit de hoofdmap van de opslagplaats de volgende opdrachten uit:

```azurecli
az webapp up
```

Deze opdracht maakt gebruik van de parameters in de cache van het bestand *.azure/config*. Omdat App Service detecteert dat de app al bestaat, wordt de code gewoon opnieuw geïmplementeerd.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

### <a name="rerun-migrations-in-azure"></a>Migraties opnieuw uitvoeren in Azure

Omdat u wijzigingen hebt aangebracht in het gegevensmodel, moet u databasemigraties opnieuw uitvoeren in App Service.

Open opnieuw een SSH-sessie in de browser door naar `https://<app-name>.scm.azurewebsites.net/webssh/host` te gaan. Voer vervolgens de volgende opdrachten uit:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

### <a name="review-app-in-production"></a>Apps in productie controleren

Ga naar `http://<app-name>.azurewebsites.net` en test de app opnieuw in productie. (Aangezien u enkel de lengte van een databaseveld hebt gewijzigd, wordt de wijziging pas zichtbaar wanneer u een langer antwoord invoert tijdens het maken van een vraag.)

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

## <a name="stream-diagnostic-logs"></a>Diagnostische logboeken streamen

U hebt toegang tot de consolelogboeken die zijn gegenereerd vanuit de container die de app host in Azure.

Voer de volgende Azure CLI-opdracht uit om de logboekstream weer te geven. Deze opdracht maakt gebruik van parameters in de cache van het bestand *.azure/config*.

```azurecli
az webapp log tail
```

Als u de consolelogboeken niet meteen ziet, probeert u het opnieuw na 30 seconden.

U kunt op elk gewenst moment stoppen met logboekstreaming door **Ctrl**+**C** te typen.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

> [!NOTE]
> U kunt ook de logboekbestanden van de browser inspecteren op `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.
>
> Met `az webapp up` worden de standaardlogboeken voor u ingeschakeld. In verband met de prestaties worden deze logboeken na bepaalde tijd vanzelf uitgeschakeld, maar ze worden steeds weer ingeschakeld wanneer u `az webapp up` opnieuw uitvoert. Voer de volgende opdracht uit om de logboeken handmatig in te schakelen:
>
> ```azurecli
> az webapp log config --docker-container-logging filesystem
> ```

## <a name="manage-your-app-in-the-azure-portal"></a>De app beheren in Azure Portal

Zoek in het [Azure-portal](https://portal.azure.com) naar de appnaam en selecteer de app in de resultaten.

![Naar uw Python Django-app navigeren in Azure Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Het portal laat standaard de pagina **Overzicht** van uw app zien; hier vindt u een algemeen overzicht van de prestaties. Hier kunt u ook algemene beheertaken uitvoeren zoals bladeren, stoppen, opnieuw starten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuratiepagina's die u kunt openen.

![Uw Python Django-app beheren op de overzichtspagina in Azure Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

## <a name="clean-up-resources"></a>Resources opschonen

Als u de app wilt behouden of wilt doorgaan naar de extra zelfstudies, sla dit dan over en ga naar [Volgende stappen](#next-steps). Zo niet, dan kunt u de resourcegroep die u gemaakt hebt voor deze zelfstudie verwijderen om doorlopende kosten te vermijden:

```azurecli
az group delete --no-wait
```

Voor de opdracht wordt de resourcegroepnaam gebruikt die in het bestand *.azure/config* in de cache is opgeslagen. Door de resourcegroep te verwijderen, kunt u ook de toewijzing van alle resources erin ongedaan maken en deze verwijderen.

Alle resources verwijderen kan enige tijd duren. Het argument `--no-wait` kan de opdracht onmiddellijk retourneren.

[Ondervindt u problemen? Laat het ons weten.](https://aka.ms/DjangoCLITutorialHelp)

## <a name="next-steps"></a>Volgende stappen

Ontdek hoe u een aangepaste DNS-naam kunt toewijzen aan uw app:

> [!div class="nextstepaction"]
> [Zelfstudie: Een aangepaste DNS-naam aan uw app toewijzen](app-service-web-tutorial-custom-domain.md)

Ontdek hoe App Service een Python-app uitvoert:

> [!div class="nextstepaction"]
> [Python-app configureren](configure-language-python.md)
