---
title: Bekende problemen en probleemoplossing
titleSuffix: Azure Machine Learning service
description: Een lijst van de bekende problemen, oplossingen, en probleemoplossing voor Azure Machine Learning-service.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 80bb7af0f7ed20336ab08d4f3ca9639057b9c67f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65149755"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekende problemen en oplossen van problemen met Azure Machine Learning-service

Dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten opgetreden bij het gebruik van de Azure Machine Learning-service.

## <a name="visual-interface-issues"></a>Problemen met de visuele interface

Visuele interface voor machine learning-problemen met de service.

### <a name="long-compute-preparation-time"></a>Lange voorbereidingstijd

Maken van nieuwe berekening of beantwoorden verlaten rekentijd duurt, is mogelijk een paar minuten of zelfs nog langer. Het team werkt voor optimalisatie.


### <a name="cannot-run-an-experiment-only-contains-dataset"></a>Een experiment uitvoeren kan niet alleen bevat gegevensset 

Om uit te voeren kunt u een experiment bevat alleen de gegevensset voor het visualiseren van de gegevensset. Maar het is niet toegestaan om uit te voeren alleen bevat gegevensset vandaag nog een experiment. We zijn actief in dit probleem is opgelost.
 
Voordat u de oplossing kunt u de gegevensset verbinden met een transformatie gegevensmodule (Select Columns in Dataset, metagegevens bewerken, Split Data enz.) en voer het experiment uit. Vervolgens kunt u de gegevensset visualiseren. 

Onderstaande afbeelding toont hoe: ![visulize-gegevens](./media/resource-known-issues/aml-visualize-data.png)

## <a name="sdk-installation-issues"></a>Problemen met de SDK-installatie

**Foutbericht: Cannot uninstall 'PyYAML'**

Azure Machine Learning-SDK voor Python: PyYAML is een geïnstalleerde distutils-project. We kan geen nauwkeurig bepalen welke bestanden behoren toe als er een gedeeltelijke verwijderen. Gebruik het volgende om door te gaan met het installeren van de SDK tijdens deze fout negeren:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problemen bij het maken van Azure Machine Learning-Computing

Er wordt een zeldzaam kans dat sommige gebruikers die hun Azure Machine Learning-werkruimte hebt gemaakt vanuit de Azure-portal voordat u de GA-versie niet mogelijk te maken van Azure Machine Learning-Computing in de werkruimte. U kunt een ondersteuningsaanvraag indient voor de service te verhogen of een nieuwe werkruimte via de Portal of de SDK om de blokkering zelf onmiddellijk te maken.

## <a name="image-building-failure"></a>Installatiekopie samenstellen fout

De installatiekopie van het bouwen van fout bij het implementeren van web-service. Tijdelijke oplossing is om toe te voegen ' pynacl 1.2.1 == "als een afhankelijkheid pip Conda-bestand voor de configuratie van installatiekopie.

## <a name="deployment-failure"></a>Fout bij de implementatie

Als u merkt `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`, het wijzigen van de SKU voor virtuele machines die worden gebruikt in uw implementatie die meer geheugen heeft.

## <a name="fpgas"></a>FPGA 's

Niet mogelijk om te implementeren van modellen op FPGA's totdat u hebt aangevraagd en goedgekeurd voor FPGA quotum. Vul het aanvraagformulier voor de quota voor het aanvragen van toegang: https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Geautomatiseerde Machine Learning

Tensor Flow geautomatiseerde machine learning biedt momenteel geen ondersteuning voor tensor flow versie 1.13. Installatie van deze versie zorgt ervoor dat pakketafhankelijkheden niet meer werken. Er wordt gewerkt om dit probleem opgelost in een toekomstige release. 

### <a name="experiment-charts"></a>Experiment grafieken

Binaire classificatie grafieken (precisie-/ oproepdiagram, ROC, krijgen curve enzovoort) die wordt weergegeven in geautomatiseerde ML-iteraties zijn niet corectly rendering in de gebruikersinterface sinds 4/12. Grafiek grafieken zijn momenteel inverse resultaten weergeven, waar betere presterende modellen met lagere resultaten worden weergegeven. Een oplossing wordt onderzocht.

## <a name="databricks"></a>Databricks

Problemen met Databricks en Azure Machine Learning.

### <a name="failure-when-installing-packages"></a>Fout bij het installeren van pakketten

Azure Machine Learning-SDK-installatie mislukt op Azure Databricks wanneer meer pakketten zijn geïnstalleerd. Sommige pakketten, zoals `psutil`, kan leiden tot conflicten. Om installatiefouten te voorkomen,-pakketten te installeren door de versie van de bibliotheek bevriezing. Dit probleem is gerelateerd aan Databricks en niet aan de Azure Machine Learning-service-SDK. U kunt dit probleem met andere bibliotheken te ervaren. Voorbeeld:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

U kunt ook init scripts gebruiken als u problemen met Python-bibliotheken installeren houden aangesloten. Deze methode wordt niet officieel ondersteund. Zie voor meer informatie, [clusterbereik init scripts](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-machine-learning-run"></a>Een geautomatiseerde machine learning-uitvoering annuleren

Wanneer u geautomatiseerde machine learning-mogelijkheden op Azure Databricks, een uitvoering annuleren en start een nieuw experiment uitvoert, start opnieuw op uw Azure Databricks-cluster.

### <a name="10-iterations-for-automated-machine-learning"></a>> 10 iteraties voor geautomatiseerde machine learning

In geautomatiseerde machine learning-instellingen, hebt u meer dan 10 iteraties instellen `show_output` naar `False` wanneer u de uitvoering verzenden.

### <a name="widget-for-the-azure-machine-learning-sdkautomated-machine-learning"></a>Widget voor de SDK/geautomatiseerde Azure Machine Learning machine learning

De SDK van Azure Machine Learning-widget wordt niet ondersteund in een Databricks-notebook omdat de notebooks kunnen HTML-widgets niet parseren. U kunt de widget in de portal bekijken met behulp van deze Python-code in uw Azure Databricks-notebook cel:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>Fout bij het importeren: Er is geen module met de naam 'pandas.core.indexes'

Als deze fout wordt weergegeven wanneer u gebruikt machine learning geautomatiseerd:

1. Voer deze opdracht uit twee om pakketten te installeren in uw Azure Databricks-cluster: 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Loskoppelen en vervolgens weer aan het cluster op uw laptop. 

Als deze stappen het probleem niet oplossen, probeert u het cluster opnieuw op te starten.

## <a name="azure-portal"></a>Azure Portal

Als u rechtstreeks naar het weergeven van uw werkruimte van een koppeling voor het delen van de SDK of de portal gaat, wordt het niet mogelijk om de normale overzichtspagina met abonnementsgegevens in de extensie weer te geven. U wordt ook niet mogelijk om over te schakelen naar een andere werkruimte. Als u nodig hebt om een andere werkruimte weer te geven, de tijdelijke oplossing is het gaat u rechtstreeks naar de [Azure-portal](https://portal.azure.com) en zoek de naam van de werkruimte.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

Soms kan het handig zijn als u diagnostische gegevens opgeven kunt wanneer u hulp vragen. Sommige Logboeken vindt u [Azure-portal](https://portal.azure.com) en Ga naar uw werkruimte en selecteer **werkruimte > Experiment > uitvoeren > Logboeken**.

## <a name="resource-quotas"></a>Resourcequota

Meer informatie over de [resourcequota](how-to-manage-quotas.md) optreden tijdens het werken met Azure Machine Learning.

## <a name="authentication-errors"></a>Verificatiefouten

Als u een bewerking voor het beheer van een compute-doel van een externe taak uitvoert, ontvangt u een van de volgende fouten:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Bijvoorbeeld, ontvangt u een foutmelding als u probeert te maken of koppelen van een compute-doel van een ML-pijplijn die wordt verzonden voor uitvoering op afstand.
