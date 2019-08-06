---
title: een query uitvoeren op gegevens met behulp van de Python-bibliotheek voor Azure Data Explorer
description: In dit artikel leert u hoe u gegevens kunt opvragen uit Azure Data Explorer met behulp van python.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 2c32e67bb2b47a24036a341ea4e1b83037abbaee
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68827528"
---
# <a name="query-data-using-the-azure-data-explorer-python-library"></a>een query uitvoeren op gegevens met behulp van de Python-bibliotheek voor Azure Data Explorer

Azure Data Explorer is een snelle en zeer schaalbare service om gegevens in logboeken en telemetrie te verkennen. Azure Data Explorer biedt een [gegevens-clientbibliotheek voor Python](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data). Met behulp van deze bibliotheek kunt u gegevens doorzoeken vanuit uw code. In dit artikel maakt u verbinding met een tabel in het *Help-cluster* die u hebt ingesteld voor hulp bij het leren. U voert vervolgens een query uit op dat cluster, waarna de resultaten worden geretourneerd.

Dit artikel is ook beschikbaar als [Azure-notebook](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueryKusto.ipynb).

## <a name="prerequisites"></a>Vereisten

* Een organisatie-e-mailaccount dat is gekoppeld aan Azure Active Directory (AAD)

* [Python](https://www.python.org/downloads/) is geïnstalleerd op uw ontwikkelcomputer

## <a name="install-the-data-library"></a>De gegevensbibliotheek installeren

Installeer *azure-kusto-data*.

```
pip install azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Importinstructies en constanten toevoegen

Importeer klassen uit de bibliotheek, evenals *pandas*, een bibliotheek voor gegevensanalyse.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
from azure.kusto.data.helpers import dataframe_from_result_table
import pandas as pd
```

Azure Data Explorer maakt gebruik van uw AAD-tenant-id om een toepassing te verifiëren. Om uw tenant-id te vinden, gebruikt u de volgende URL, waarbij u *YourDomain* vervangt door uw domeinnaam.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Dus als uw domein *contoso.com* is, wordt de URL bijvoorbeeld: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Klik op deze URL om de resultaten weer te geven. De eerste regel is als volgt.

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

De tenant-id is in dit geval `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. Stel de waarde voor AAD_TENANT_ID in voordat u deze code uitvoert.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_CLUSTER = "https://help.kusto.windows.net/"
KUSTO_DATABASE = "Samples"
```

Nu maakt u de verbindingsreeks. In dit voorbeeld wordt apparaatverificatie gebruikt voor toegang tot het cluster. U kunt ook een [AAD-toepassingscertificaat](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L24), een [AAD-toepassingssleutel](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L20) en een [AAD-gebruiker en -wachtwoord](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L34) gebruiken.

```python
KCSB = KustoConnectionStringBuilder.with_aad_device_authentication(
    KUSTO_CLUSTER)
KCSB.authority_id = AAD_TENANT_ID
```

## <a name="connect-to-azure-data-explorer-and-execute-a-query"></a>Verbinding maken met Azure Data Explorer en een query uitvoeren

Voer een query uit op het cluster en sla de uitvoer op in een gegevensframe. Wanneer deze code wordt uitgevoerd, wordt een bericht als het volgende geretourneerd: *Als u zich wilt aanmelden, opent u de pagina https://microsoft.com/devicelogin met een webbrowser. Voer de code F3W4VWZDM in om te verifiëren*. Volg de stappen om u aan te melden en ga vervolgens terug om het volgende codeblok uit te voeren.

```python
KUSTO_CLIENT = KustoClient(KCSB)
KUSTO_QUERY = "StormEvents | sort by StartTime desc | take 10"

RESPONSE = KUSTO_CLIENT.execute(KUSTO_DATABASE, KUSTO_QUERY)
```

## <a name="explore-data-in-dataframe"></a>Gegevens verkennen in DataFrame

Nadat u zich hebt aangemeld, retourneert de query resultaten en worden deze opgeslagen in een gegevensframe. U kunt op dezelfde manier met deze resultaten werken als met andere gegevensframes.

```python
df = dataframe_from_result_table(RESPONSE.primary_results[0])
df
```

Als het goed is, worden de bovenste tien resultaten uit de tabel StormEvents weergegeven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gegevens opnemen met behulp van de Azure Data Explorer python-bibliotheek](python-ingest-data.md)
