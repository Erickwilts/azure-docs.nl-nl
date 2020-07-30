---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: c6a9f17d46ef8feb571c0ecc7a0a93a169f74725
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285544"
---
De eenvoudigste manier om een Python-cloudtoepassing te verifiëren, is met een beheerde identiteit. Zie [Een door App Service beheerde identiteit gebruiken om toegang te krijgen tot Azure Key Vault](/azure/key-vault/general/managed-identity) voor meer informatie. 

In deze quickstart wordt echter een desktoptoepassing gemaakt, omdat dit eenvoudiger is. Voor deze toepassing is het gebruik van een service-principal en een toegangsbeheerbeleid vereist. Voor uw service-principal is een unieke naam vereist met de notatie http://&lt;mijn-unieke-service-principal-naam&gt;.

Maak een service-principal met behulp van de Azure CLI-opdracht [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac):

```azurecli
az ad sp create-for-rbac -n "http://<my-unique-service-principal-name>" --sdk-auth
```

Met deze bewerking wordt een reeks sleutel-waardeparen geretourneerd.

```console
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
