---
title: Verificatie geheimen in trainingen
titleSuffix: Azure Machine Learning
description: Informatie over het door geven van geheimen aan trainings uitvoeringen op veilige wijze met behulp van de Azure Key Vault voor uw werk ruimte.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.date: 03/09/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: ce47041d888b8a7786f0f87f54e725919638e7f7
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93349015"
---
# <a name="use-authentication-credential-secrets-in-azure-machine-learning-training-runs"></a>Verificatie referentie geheimen gebruiken in Azure Machine Learning training-uitvoeringen


In dit artikel leert u hoe u geheimen kunt gebruiken in de training die veilig worden uitgevoerd. Verificatie-informatie, zoals uw gebruikers naam en wacht woord, zijn geheimen. Als u bijvoorbeeld verbinding maakt met een externe data base om trainings gegevens op te vragen, moet u uw gebruikers naam en wacht woord door geven aan de context voor externe uitvoering. Het coderen van dergelijke waarden in trainings scripts is onveilig, omdat het geheim zou worden weer gegeven. 

In plaats daarvan bevat uw Azure Machine Learning-werk ruimte een gekoppelde resource met de naam [Azure Key Vault](../key-vault/general/overview.md). Gebruik deze Key Vault om geheimen door te geven aan externe uitvoering van een set Api's in de Azure Machine Learning python SDK.

De standaard stroom voor het gebruik van geheimen is:
 1. Meld u op de lokale computer aan bij Azure en maak verbinding met uw werk ruimte.
 2. Stel op lokale computer een geheim in de werk ruimte Key Vault in.
 3. Een externe uitvoering verzenden.
 4. Haal het geheim op in de externe uitvoering van Key Vault en gebruik het.

## <a name="set-secrets"></a>Geheimen instellen

In de Azure Machine Learning bevat de klasse sleutel [kluis](/python/api/azureml-core/azureml.core.keyvault.keyvault?preserve-view=true&view=azure-ml-py) methoden voor het instellen van geheimen. In uw lokale python-sessie moet u eerst een verwijzing naar uw werk ruimte verkrijgen Key Vault en vervolgens de [`set_secret()`](/python/api/azureml-core/azureml.core.keyvault.keyvault?preserve-view=true&view=azure-ml-py#&preserve-view=trueset-secret-name--value-) methode gebruiken om een geheim op naam en waarde in te stellen. De __set_secret__ methode werkt de geheime waarde bij als de naam al bestaat.

```python
from azureml.core import Workspace
from azureml.core import Keyvault
import os


ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Plaats de geheime waarde niet in uw Python-code omdat deze onveilig is om deze in het bestand op te slaan als een lees bare tekst. Haal in plaats daarvan de geheime waarde op uit een omgevings variabele, bijvoorbeeld Azure DevOps build Secret of van interactieve gebruikers invoer.

U kunt geheime namen vermelden met behulp van de- [`list_secrets()`](/python/api/azureml-core/azureml.core.keyvault.keyvault?preserve-view=true&view=azure-ml-py#&preserve-view=truelist-secrets--) methode en er is ook een batch versie,[set_secrets ()](/python/api/azureml-core/azureml.core.keyvault.keyvault?preserve-view=true&view=azure-ml-py#&preserve-view=trueset-secrets-secrets-batch-) waarmee u meerdere geheimen tegelijk kunt instellen.

## <a name="get-secrets"></a>Geheimen ophalen

In uw lokale code kunt u de- [`get_secret()`](/python/api/azureml-core/azureml.core.keyvault.keyvault?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-secret-name-) methode gebruiken om de geheime waarde op naam op te halen.

Voor uitvoeringen verzonden [`Experiment.submit`](/python/api/azureml-core/azureml.core.experiment.experiment?preserve-view=true&view=azure-ml-py#&preserve-view=truesubmit-config--tags-none----kwargs-)  , gebruikt u de- [`get_secret()`](/python/api/azureml-core/azureml.core.run.run?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-secret-name-) methode met de- [`Run`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py) klasse. Omdat een ingediende uitvoering op de hoogte is van de werk ruimte, wordt met deze methode de activering van de werk ruimte versneld en wordt de geheime waarde direct geretourneerd.

```python
# Code in submitted run
from azureml.core import Experiment, Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Zorg ervoor dat u de geheime waarde niet beschikbaar maakt door deze te schrijven of af te drukken.

Er is ook een batch versie, [get_secrets ()](/python/api/azureml-core/azureml.core.run.run?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-secrets-secrets-) voor het openen van meerdere geheimen tegelijk.

## <a name="next-steps"></a>Volgende stappen

 * [Voorbeeld notitieblok weer geven](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Meer informatie over Enter prise Security met Azure Machine Learning](concept-enterprise-security.md)