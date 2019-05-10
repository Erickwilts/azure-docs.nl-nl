---
title: Maken en beheren van lezen-replica's in Azure Database for MySQL
description: Dit artikel wordt beschreven hoe u kunt instellen en beheren van lezen-replica's in Azure Database voor MySQL met behulp van de Azure CLI.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: a9ca34953827c1f94e2696eb4f09163be335d2f4
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510688"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli"></a>Over het maken en beheren lezen-replica's in Azure Database for MySQL met behulp van de Azure CLI

In dit artikel leert u hoe u kunt maken en beheren van meer replica's binnen dezelfde Azure-regio als het model in de Azure Database for MySQL-service met de Azure CLI.

> [!NOTE]
> Nog biedt niet maken replica's in een andere regio van de hoofd-server ondersteuning voor Azure CLI. Gebruik voor het maken van een replica van de regio-overschrijdende de [Azure-portal]( howto-read-replicas-portal.md) in plaats daarvan.

## <a name="prerequisites"></a>Vereisten

- [Installeer Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Een [Azure Database for MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md) die wordt gebruikt als de hoofd-server. 

> [!IMPORTANT]
> De functie lezen replica is alleen beschikbaar voor Azure Database voor MySQL-servers in de Prijscategorieën voor algemeen gebruik of geoptimaliseerd voor geheugen. Controleer of de hoofd-server in een van deze Prijscategorieën.

## <a name="create-a-read-replica"></a>Maken van een replica lezen

Een lezen-replica-server kan worden gemaakt met de volgende opdracht:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

De `az mysql server replica create` opdracht moet de volgende parameters:

| Instelling | Voorbeeldwaarde | Description  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waar de replica-server om te worden gemaakt.  |
| naam | mydemoreplicaserver | De naam van de nieuwe replicaserver die is gemaakt. |
| source-server | mydemoserver | De naam of ID van de bestaande hoofd-server om te repliceren van. |

> [!NOTE]
> Lezen-replica's worden gemaakt met de configuratie van de dezelfde server als de master. De configuratie van de replica-server kan worden gewijzigd nadat deze is gemaakt. Het wordt aanbevolen dat de configuratie van de replica-server moet worden opgeslagen op de waarden gelijk zijn aan of groter zijn dan het model om te controleren of dat de replica kan houden met de master.

## <a name="stop-replication-to-a-replica-server"></a>Replicatie naar een replica-server stoppen

> [!IMPORTANT]
> Replicatie naar een server stoppen is niet ongedaan worden gemaakt. Wanneer u replicatie tussen een model en de replica is gestopt, kunnen deze kan niet ongedaan worden gemaakt. De replica-server vervolgens wordt een zelfstandige server en biedt nu ondersteuning voor zowel lees- en schrijfbewerkingen. Deze server kan niet opnieuw worden gemaakt in een replica.

Replicatie naar een lezen-replica-server kan worden gestopt met de volgende opdracht:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

De `az mysql server replica stop` opdracht moet de volgende parameters:

| Instelling | Voorbeeldwaarde | Description  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waarin de replica-server bestaat.  |
| naam | mydemoreplicaserver | De naam van de replica-server om replicatie te stoppen op. |

## <a name="delete-a-replica-server"></a>Een replica-server verwijderen

Verwijderen van een lezen-replica-server kan worden gedaan door het uitvoeren van de **[az mysql server delete](/cli/azure/mysql/server)** opdracht.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

## <a name="delete-a-master-server"></a>Een hoofd-server verwijderen

> [!IMPORTANT]
> Verwijderen van een hoofd-server-replicatie naar alle replicaservers stopt en Hiermee verwijdert u de hoofd-server zelf. Replica-servers worden zelfstandige servers die bieden nu ondersteuning voor zowel lees- en schrijfbewerkingen.

Als u wilt verwijderen een hoofd-server, kunt u uitvoeren de **[az mysql server delete](/cli/azure/mysql/server)** opdracht.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="list-replicas-for-a-master-server"></a>Lijst met replica's voor een hoofd-server

Als u wilt weergeven van alle replica's voor een bepaalde hoofd-server, moet u de volgende opdracht uitvoeren: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

De `az mysql server replica list` opdracht moet de volgende parameters:

| Instelling | Voorbeeldwaarde | Description  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waar de replica-server om te worden gemaakt.  |
| servernaam | mydemoserver | De naam of ID van de hoofd-server. |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [lezen replica's](concepts-read-replicas.md)
